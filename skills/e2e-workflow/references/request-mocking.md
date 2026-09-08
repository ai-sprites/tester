# 响应模拟与局部补丁

当分支依赖难以稳定、安全构造的服务端状态时读取。mock 的用途和证明范围必须进入计划；优先保持导航、输入、搜索、筛选和渲染流程真实，不能模拟掉本次要验证的行为。入口见 [E2E 工作流](../SKILL.md)。

## 先确定边界

- 能通过已授权、隔离的 UI 或数据准备流程建立的状态，优先使用该流程。仅为必要的服务端分支设置窄范围响应模拟，说明真实数据无法满足条件的原因。
- 先从已确认契约或脱敏真实响应了解结构、身份、分页、状态和错误字段，再改变分支需要的字段。不能凭想象拼响应，不能擅自改变身份、认证或真实账号权限。
- 浏览器响应补丁可以验证客户端在模拟条件下的行为，不能证明服务端权限控制、真实写入或端到端持久化。相关验收需要另有真实证据。

## 路由与请求的顺序

按 [可靠性与就绪信号](reliability-and-readiness.md) 先安装 route、订阅响应，再触发请求并等待界面结果。路由需按已核实的路径、method 和必要参数/实体收窄，避免 `**/*` 或只看名称片段的宽匹配。框架的 URL pattern 未包含 method 时，在 handler 内检查 method 并对无关请求透传。

每个测试独立设置并释放自己的模拟，或使用项目已有、能保证隔离的 fixture。不要清除其他测试/fixture 的全部 route，也不要遗留全局 handler。服务工作线程或其他拦截层使 route 无法命中时，先查真实请求路径和项目配置，不把未命中当成成功模拟。

## 何时可以转发真实请求

`route.fetch()` 或同类透传会真的访问服务端。仅在语义上无副作用的安全读取时取回真实响应并打补丁；不能只按 HTTP method 名称断言请求安全。

对于真实写入，只有它本就是已授权、隔离且有恢复方法的测试步骤时才发送。否则用已确认的响应样本直接 fulfill，不先发送写请求再试图覆盖结果。模拟成功写入只能支持客户端反馈的结论。

## 改写响应时保留契约

1. 检查实际 content type；非 JSON 响应不强行解析，可按原响应透传。异常状态是否应保留或模拟由本条计划的预期决定，不统一改成 `200`。
2. 解析已知结构，定位特定实体，只修改需要的字段。目标不存在或形状不符时明确失败并排查，不静默跳过补丁后仍声称 mock 生效。
3. 序列化修改后的内容，显式提供正确 status、content type 和 body。保留应用需要的语义 headers；重算或移除已经失效的 content length、content encoding 等传输元信息，不能沿用压缩前后不匹配的 headers。
4. 证明目标 route 实际命中，等待补丁响应及其界面结果。断言应能区分期望模拟状态与未修改的真实状态，不能只检查页面仍可见。

测试结束保留 mock 原因、修改字段、真实/模拟边界和结果证据到 `docs/tester/<feature-id>/`。原始账号数据、认证 headers 和响应中的秘密不进入报告或提交；一次性捕获文件按 [探索工具](exploration-tooling.md) 清理。

## 例：只补丁一个安全读取的响应

下面假设已确认 `/api/items/item-1` 是无副作用的 GET，响应含 `id` 和 `availability`。这些只是示例；实际端点、字段与状态须来自目标计划和捕获的契约。路由在触发请求的导航前安装，并在该测试结束时按 fixture 生命周期清理。

```ts
await page.route("**/api/items/item-1", async (route) => {
  if (route.request().method() !== "GET") {
    await route.continue();
    return;
  }
  const response = await route.fetch();
  if (!response.headers()["content-type"]?.includes("json")) {
    await route.fulfill({ response });
    return;
  }
  const payload = await response.json();
  if (payload.id !== "item-1") throw new Error("Unexpected fixture response");
  payload.availability = "unavailable";

  const headers = { ...response.headers() };
  delete headers["content-length"];
  delete headers["content-encoding"];
  await route.fulfill({
    status: response.status(),
    headers,
    contentType: "application/json",
    body: JSON.stringify(payload),
  });
});
```

测试仍须等待补丁响应并断言特定的 unavailable 界面状态，证明 route 确实命中；页面可见或真实响应透传本身不能算模拟成功。写请求不使用这段 `route.fetch()` 模式，除非真实写入本就是已授权、隔离的测试步骤。
