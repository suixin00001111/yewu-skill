# ui组业务组件 · 异步状态与接口占位

保证生成页可接后台：有加载、空、错、写操作反馈，而不是只有静态表格。

---

## 1. 读操作（列表/详情）

| 状态 | UI | 说明 |
|------|-----|------|
| loading | 区域 Spin 或 Skeleton | 首屏/翻页/筛选查询 |
| success + 有数据 | 正常 Table/Descriptions | |
| success + 无数据 | Empty + 可选主按钮（新建/重置筛选） | 文案可配置 |
| error | Alert 或 Result 区 +「重试」 | Message.error 可选补充 |

列表查询按钮：请求中 `loading`，结束后关闭。

## 2. 写操作（保存/删除/提交）

```
用户操作 →（危险则先确认 Modal/Popconfirm）
  → 主按钮 loading 或 Message.loading
  → 成功：Message.success + 刷新/跳转
  → 失败：Message.error + 解除 loading
```

- 删除：**确认 → 成功 Message**（文案可配置，默认可「删除成功」）  
- 禁止：成功后再弹成功 Modal；禁止静默成功  

## 3. 接口占位约定

代码中预留服务函数（名称按业务改），页面内不写死完整 URL 也可：

```ts
// services/xxx.ts（占位）
export async function fetchList(params: ListQuery): Promise<PageResult<Item>> {
  // TODO: GET /api/...
  throw new Error('not implemented');
}
export async function saveItem(id: string, body: ItemForm): Promise<void> {
  // TODO: PUT /api/...
}
export async function removeItem(id: string): Promise<void> {
  // TODO: DELETE /api/...
}
```

页面只依赖上述函数；演示可用 mock 数组。

## 4. 分页与筛选

- 筛选「查询」触发 `fetchList`；「重置」清空条件并重新拉  
- Pagination 变更 page/size 后重新请求  
- 查询参数与 Table 数据源单一状态，避免两套数据  

## 5. 自检

- [ ] 有 loading  
- [ ] 有 empty  
- [ ] 有 error/重试或失败 Message  
- [ ] 写操作有成功/失败 Message  
- [ ] 存在 services 占位或 TODO 接口注释  
