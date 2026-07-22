# ui组业务组件 · 侧栏折叠 Sider

生成带侧栏的中后台页时**默认实现可折叠侧栏**，且必须符合本节。  
对齐实现层 `Layout.Sider` + `Menu` 折叠能力；界面不出现第三方库品牌名。

---

## 0. 生成时强制结论（先读）

| 项 | 必须 |
|----|------|
| 展开宽 | **220px**（允许 200–220） |
| 收起宽 | **48px**（`collapsedWidth={48}`） |
| 收起内容 | **仅图标**居中；文案隐藏；`title`/Tooltip 显示菜单名 |
| 折叠入口 | **仅侧栏底部 1 个** trigger（库自带即可） |
| 滚动 | **仅菜单区**内部 `overflow-y:auto`；Sider 容器 `overflow:hidden` |
| 布局 | Header 通栏在上；其下 `Sider + Content` 横向 flex；Content `flex:1; min-width:0` |
| 禁止 | 侧栏与内容之间的灰滚动轨、双折叠按钮、默认 resize 拖拽条、收起后仍留 200px 空白 |

**坏例特征（交付直接打回）**：中间一条纵向灰轨 + 上下三角；底栏折叠与中间把手并存；收起后菜单文字仍挤在窄条里换行。

---

## 1. 结构

```
┌──────── Header 通栏 #004EA2 ────────┐
├─ Sider ──┬──────── Content ─────────┤
│  Logo?   │  业务区 overflow:auto    │
│  Menu↑   │                          │
│  (scroll)│                          │
│  Trigger │                          │
└──────────┴──────────────────────────┘
```

- Sider：`display:flex; flex-direction:column; overflow:hidden; flex-shrink:0`
- Menu 外包一层或 Menu 自身：`flex:1; min-height:0; overflow-x:hidden; overflow-y:auto`
- Trigger：`flex-shrink:0` 贴底，高度约 40–48px，顶部分割线
- **不要**在 Sider 外包可滚动 div，**不要**在 Sider 与 Content 之间再插一列

---

## 2. 实现层代码（默认模板）

```tsx
import { useState } from 'react';
import { Layout, Menu } from '@/components'; // 实现层入口
const { Header, Sider, Content } = Layout;

export function AdminShell({ children, selectedKeys }) {
  const [collapsed, setCollapsed] = useState(false);

  return (
    <Layout style={{ height: '100vh' }}>
      <Header style={{ background: '#004EA2', color: '#fff', height: 52 }}>
        {/* 系统名 + 工具区；顶栏通栏，不随侧栏变窄 */}
      </Header>
      <Layout hasSider style={{ minHeight: 0, flex: 1 }}>
        <Sider
          theme="light"
          collapsible
          collapsed={collapsed}
          onCollapse={setCollapsed}
          width={220}
          collapsedWidth={48}
          // 禁止：resizeDirections / 自定义双 trigger 叠加
          style={{
            background: '#fff',
            borderRight: '1px solid var(--color-border-2)',
            overflow: 'hidden',
          }}
        >
          <Menu
            selectedKeys={selectedKeys}
            collapse={collapsed} // 必须与 collapsed 同步
            style={{ height: '100%', borderRight: 0 }}
          >
            <Menu.Item key="workbench" icon={<IconHome />}>
              工作台
            </Menu.Item>
            <Menu.Item key="list" icon={<IconList />}>
              列表
            </Menu.Item>
          </Menu>
        </Sider>
        <Content style={{ flex: 1, minWidth: 0, overflow: 'auto', background: 'var(--color-fill-2)', padding: 16 }}>
          {children}
        </Content>
      </Layout>
    </Layout>
  );
}
```

### 同步规则（强制）

1. `Menu` 的 `collapse={collapsed}` **等于** `Sider` 的 `collapsed`  
2. 每个 `Menu.Item` **必须有 icon**（IconPark outline），否则收起后不可识别  
3. 选中态：展开/收起均为底 `#E6F1FF` + 主色图标（字）  
4. 需要记忆折叠：可用 `localStorage`（键名自定，如 `ui-sider-collapsed`），勿写进页面可见文案  

### 不要默认开启

- `resizeDirections` / 可拖拽改宽（易出现中间灰轨；用户明确要求再开）  
- 自定义浮动折叠钮 + 库 trigger 同时存在  
- 给 Sider 设 `overflow: auto`（滚动条会跑到错误位置）

---

## 3. 纯 CSS/静态页骨架

```css
.app { display: flex; flex-direction: column; height: 100vh; }
.header { height: 52px; flex-shrink: 0; background: #004EA2; }
.body { display: flex; flex: 1; min-height: 0; min-width: 0; }
.sider {
  width: 220px;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  background: #fff;
  border-right: 1px solid #E5E6EB;
  overflow: hidden;
  transition: width 0.2s ease;
}
.sider.is-collapsed { width: 48px; }
.sider-menu {
  flex: 1;
  min-height: 0;
  overflow-x: hidden;
  overflow-y: auto;
  padding: 8px;
}
.menu-item {
  display: flex;
  align-items: center;
  gap: 10px;
  height: 40px;
  padding: 0 12px;
  border-radius: 4px;
  width: 100%;
  border: 0;
  background: transparent;
  color: #4E5969;
  cursor: pointer;
}
.menu-item.active { background: #E6F1FF; color: #165DFF; }
.sider.is-collapsed .menu-item {
  width: 32px;
  height: 32px;
  margin: 4px auto;
  padding: 0;
  justify-content: center;
  gap: 0;
}
.sider.is-collapsed .menu-label { display: none; }
.sider-trigger {
  flex-shrink: 0;
  height: 40px;
  border: 0;
  border-top: 1px solid #E5E6EB;
  background: #fff;
  cursor: pointer;
  width: 100%;
}
.main { flex: 1; min-width: 0; overflow: auto; }
```

---

## 4. 与壳层关系

| 元素 | 折叠时 |
|------|--------|
| 顶栏 | 仍通栏全宽 `#004EA2`，**不变窄** |
| 侧栏底 | 仍白；选中仍 `#E6F1FF` |
| 内容区 | 自动变宽；`min-width:0` 防横向撑破 |

---

## 5. 交付自检（侧栏专项）

- [ ] 展开 220 / 收起 48，transition 平滑  
- [ ] 收起后只有图标，无文字挤压换行  
- [ ] 仅一个折叠入口（底栏）  
- [ ] **没有**侧栏与内容之间的独立灰滚动轨/拖拽条  
- [ ] 菜单多时可在菜单区内滚，Sider 外无第三列滚动  
- [ ] Content `min-width:0`，整页无异常横滚  
- [ ] `Menu.collapse` 与 `Sider.collapsed` 同步；项均有 icon  

任一项否 → 按本节重做侧栏，勿只改业务内容区。
