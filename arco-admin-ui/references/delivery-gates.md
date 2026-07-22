# ui组业务组件 · 交付闸门（fail 即重做）

生成结束前逐项过；**任一项否 → 修改后再交付**。

## 壳与品牌

- [ ] 顶栏 `#004EA2` 通栏白字；侧栏白底；选中 `#E6F1FF` + 蓝字/图标
- [ ] 侧栏默认可折叠：展开~220 / 收起 48 仅图标；无侧栏与内容间独立灰滚动轨；仅一个折叠入口；Menu.collapse 已同步（见 sider-collapse）  
- [ ] 主色按钮 `#165DFF`；无外部设计系统/厂商品牌文案  
- [ ] 无 skill 规范注释、token 名刷在页面上  

## 结构

- [ ] 工作台：无内容区「工作台」大标题  
- [ ] 列表：页名单独一行；新建等在表上工具行，不与标题并列  
- [ ] 详情：页头条左标题右返回/取消/保存；页面级操作不在表格行  
- [ ] 模块 Card 内边距 16–20；Descriptions/表不贴边  

## 组件与反馈

- [ ] 表格浅灰表头；详情明细有完整描边  
- [ ] 删除等：先确认，成功后 Message（文案可配置）  
- [ ] 弹窗顶栏 `linear-gradient(180deg, #eaf0ff 0%, #ffffff 34%)`；底栏取消左主按钮右  
- [ ] 弹窗/按钮/标题文案来自用户或业务，非无关示例死文案  
- [ ] Progress 与 Steps 不混用；Steps 满宽  

## 可接后台

- [ ] 有 loading / empty（或说明为何不需要）  
- [ ] 写操作有成功/失败提示  
- [ ] 有 services 占位或 TODO 接口  

## 图标

- [ ] 业务图标 IconPark outline；非 Emoji 图标体系

## 侧栏折叠（有侧栏的页面必过）

- [ ] `collapsible` + `collapsedWidth={48}` + 展开约 220  
- [ ] `Menu` 设置 `collapse` 与 Sider 同步；菜单项均有 icon  
- [ ] 仅底部一个折叠入口  
- [ ] **无**侧栏|内容之间的灰滚动轨 / resize 拖拽条  
- [ ] Sider `overflow:hidden`，滚动只在菜单区  
- [ ] Content `min-width:0`  

