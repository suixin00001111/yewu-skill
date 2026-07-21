# ui组业务组件 · 明暗主题切换（实现层按组件库规范）

> **目标**：支持 Light / Dark 一键切换。  
> **实现层**：与项目中后台组件库的官方暗色方案对齐。  
> **对外文案**：称「明暗切换 / 深色模式」，不必强调第三方品牌名。  
> **壳约束**：顶栏品牌色 **Light/Dark 均保持** `#004EA2` + 白字；禁止把顶栏改成渐变/玻璃。

---

## 1. 官方切换方式（必须）

### React（默认）

1. 全局引入实现层组件库样式（内置明暗 CSS 变量）。
2. 切换时对 **`document.body`** 设置属性（实现层主题属性约定）：

```js
// 深色
// 主题属性名以项目实现层组件库为准（下方为常见写法示例）
document.body.setAttribute('arco-theme', 'dark');
// 浅色
document.body.removeAttribute('arco-theme');
// 或
document.body.setAttribute('arco-theme', 'light'); // 若项目统一用属性，以当前实现层文档为准；常见为 dark 有值、light 移除
```

3. 应用根节点可用 `ConfigProvider` 包一层（语言、组件默认 size 等）；**主题明暗以 body 上的实现层主题属性为主**，不要另写一套完整色板却不接组件库 CSS 变量。
4. 业务样式优先用 CSS 变量，例如：

```css
color: var(--color-text-1);
background: var(--color-bg-2);
border-color: var(--color-border-2);
```

禁止在业务组件里写死后的大段 `#fff` / `#000` 导致切暗色失效。

### Vue（仅用户指定 Vue 时）

- 同样使用 `body` 的主题属性约定（与实现层 Vue 文档一致，属性名以工程为准）。
- 样式引入对应包的 css。

### 参考（实现查阅，勿当产品名展示）

- 暗色模式：见实现层主题文档
- 设计变量：见实现层 Token / 变量文档
- 全局配置：见实现层 ConfigProvider / 全局配置文档

---

## 2. 状态与持久化

| 项 | 规范 |
|----|------|
| 默认 | **Light**（与历史后台一致） |
| 存储 | `localStorage` 键建议 `ui-theme`，值 `light` / `dark` |
| 初始化 | 首屏 JS 尽早读存储并设置实现层主题属性，减少闪白/闪黑 |
| 系统偏好 | 可选：无存储时读 `prefers-color-scheme`；**有用户选择后以用户为准** |
| 范围 | 全应用一致；不要每页各自一套未同步的 theme |

---

## 3. 入口位置（UI）

- 放在 **顶栏右侧工具区**（与通知、设置、头像同一区域）。
- 控件：图标按钮（`IconPark Sun` / `IconMoon` 或 `IconSkin`）或带 Tooltip 的切换；`size` 与顶栏其它图标一致。
- 图标/文字在顶栏上为 **白色/浅色**（顶栏始终 `#004EA2`）。
- 不要把明暗切换塞进侧栏菜单一级业务项，除非用户明确要求「设置页」里再放一份。

示例结构（示意）：

```jsx
// Header 右侧
<Button
  type="text"
  style={{ color: '#fff' }}
  icon={theme === 'dark' ? <IconSun /> : <IconMoon />}
  onClick={toggleTheme}
/>
```

---

## 4. Light / Dark 下的产品壳

### 4.1 两态都不变

| 元素 | 规则 |
|------|------|
| 顶栏背景 | `#004EA2` 通栏 |
| 顶栏文字/图标 | 白 / 浅色 |
| 主按钮 primary | `#165DFF`（组件库 primary；暗色下仍走 token，勿换成紫/绿） |
| 信息架构 | 顶栏 + 侧栏 + 内容区；工作台/列表/详情结构不因主题改版式 |

### 4.2 Light（默认，与历史规范一致）

| 元素 | 色 |
|------|-----|
| 侧栏 | 白底 `#FFFFFF` |
| 菜单选中 | 底 `#E6F1FF`，字/图标 `#165DFF` |
| 内容区底 | `#F2F3F5` / `#F7F8FA` |
| 卡片 | 白底 |

### 4.3 Dark（组件库暗色 + 壳适配）

| 元素 | 规则 |
|------|------|
| 侧栏背景 | 用暗色表面 token（如 `var(--color-menu-light-bg)` 在 dark 下的值 / `var(--color-bg-2)` / 组件库 Menu 暗色背景），**不要**假深色侧栏用纯黑营销风，也**不要**暗色里仍强行白侧栏导致刺眼 |
| 菜单默认文字 | `var(--color-text-1)` / `var(--color-text-2)` |
| 菜单选中 | 主色浅底（primary 透明或 dark 下的 primary-1 等效）+ 主色文字；**不要**继续写死 Light 的 `#E6F1FF` 大色块 |
| 内容区底 | `var(--color-bg-2)` 或 fill 级暗色底 |
| 卡片 / 表格 / 输入 | 跟组件库暗色默认，边框用 `var(--color-border-2)` |
| 页头操作条 | 暗色表面 + 清晰边框，仍「有背景容器」，不要透明贴底 |
| 工作台 | 结构仍按 content-ux；卡片用暗色 Card，图标块用暗色 fill，勿玻璃拟态 |

自定义壳样式示例（优先变量）：

```css
/* 顶栏品牌色两态固定 */
.app-header {
  background: #004EA2;
  color: #fff;
}

/* 侧栏：跟随主题 */
.app-sider {
  background: var(--color-bg-1);
  border-right: 1px solid var(--color-border-2);
}

/* Light 选中可保留产品色；Dark 勿写死浅蓝色块 */
body:not([arco-theme='dark']) .app-menu-item-selected {
  background: #E6F1FF;
  color: #165DFF;
}
body[arco-theme='dark'] .app-menu-item-selected {
  background: rgba(22, 93, 255, 0.16);
  color: rgb(var(--primary-6));
}
```

---

## 5. 生成代码时的默认行为

1. **用户提到明暗 / 深色 / 主题切换** → 必须实现：顶栏切换 + 实现层 body 主题属性 + localStorage。
2. **用户未提** → 默认 Light；壳与内容按 Light 规范。可在顶栏预留切换（推荐后台脚手架带上），或注释说明可加。
3. 演示页 / 可运行 HTML：若用 CDN 组件库，同样设置 `body` 属性并引入官方 css。
4. 出 **UI 图** 时：默认出 Light；若用户要暗色稿，注明 `dark`，顶栏仍为 `#004EA2`，内容为组件库暗色气质。

---

## 6. 自检清单

- [ ] 切换后 Table / Form / Modal / Dropdown 等组件是否一起变暗（而非只有背景变）
- [ ] 是否按实现层约定设置 body 主题属性，而非自造属性却不接组件库 CSS 变量
- [ ] 顶栏是否仍为 `#004EA2`
- [ ] Dark 下菜单选中是否可读、未写死 Light 的 `#E6F1FF`
- [ ] 业务硬编码色是否导致「半套暗色」
- [ ] 刷新后主题是否恢复（localStorage）
- [ ] 无紫渐变、玻璃拟态「暗黑炫光」皮肤

---

## 7. 与其它文档关系

| 文档 | 关系 |
|------|------|
| design-tokens.md | Light 默认色表；Dark 以组件库 CSS 变量为准 |
| product-shell.md | 顶栏位置放切换；壳结构两态共享 |
| content-ux.md | 工作台结构不变，表面色随主题 |
| do-dont.md | 禁止假暗色、禁止改顶栏品牌色 |

