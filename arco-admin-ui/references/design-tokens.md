# ui组业务组件 · Design Tokens（默认主题 Light）

实现时优先用 CSS 变量 / token，勿硬编码随意色值。下列 token 与实现层组件库默认主题对齐。  
样式原则与组件用法详见 [style-usage.md](style-usage.md)。

## 0. 产品壳固定色（优先于其它主色讨论）

| 用途 | 色值 | 说明 |
|------|------|------|
| **顶栏 Header 背景** | #004EA2 | **确定样式**，通栏深蓝，白字 |
| 顶栏文字/图标 | #FFFFFF | |
| **菜单选中背景** | #E6F1FF | **确定样式** |
| 菜单选中文字/图标 | #165DFF（可略亮如 #398EFF） | |
| 侧栏背景 | #FFFFFF | 非深色侧栏 |
| 内容区背景 | #F2F3F5 / #F7F8FA | |
| 主按钮 primary | #165DFF | 查询/添加/处理等 |

顶栏 #004EA2 与按钮主色 #165DFF **可以不同**。

## 1. 品牌与语义色

| Token 语义 | 默认值 | 说明 |
|------------|--------|------|
| primary-6（主色） | #165DFF | 主按钮、链接、选中文字、焦点 |
| primary-5 / hover | #4080FF | 主色悬停（约） |
| primary-7 / active | #0E42D2 | 主色按下（约） |
| success-6 | #00B42A | 成功 |
| warning-6 | #FF7D00 | 警告 |
| danger-6 | #F53F3F | 错误/危险/删除文字 |
| link | 同 primary | 文字链接、表格操作 |

浅背景用 #E6F1FF / #E8F3FF；行动主色、结果 success、风险 warning、破坏 danger；勿主色铺满页面底。

## 2. 文字色（四级）

| Token | 默认值 | 用途 |
|-------|--------|------|
| color-text-1 | #1D2129 | 标题、强调正文 |
| color-text-2 | #4E5969 | 次级正文 |
| color-text-3 | #86909C | 辅助说明、占位 |
| color-text-4 | #C9CDD4 | 禁用、更弱提示 |

用色阶表达层级，不要透明度硬造灰。

## 3. 填充 / 背景 / 边框

| Token | 默认值 | 用途 |
|-------|--------|------|
| color-bg-1 | #FFFFFF | 纯白内容 |
| color-bg-2 | #F7F8FA | 次级背景、表头 |
| color-fill-1 | #F7F8FA | 浅填充 |
| color-fill-2 | #F2F3F5 | 内容区底 |
| color-fill-3 | #E5E6EB | 悬停填充 |
| color-border-1 | #F2F3F5 | 极浅分割 |
| color-border-2 | #E5E6EB | 默认边框 |
| color-border-3 | #C9CDD4 | 更强边框 |
| color-border-4 | #86909C | 强调边框 |

## 3.1 明暗主题

- 默认 Light；Dark 切换 body 上实现层主题属性（代码保持库约定属性名）+ CSS 变量。
- 顶栏两态仍 #004EA2 + 白字；业务色走 ar(--color-*)。
- 细则见 [theme-mode.md](theme-mode.md)。

## 4. 字体

- 中文：PingFang SC, Microsoft YaHei, Hiragino Sans GB
- 西文/数字：Inter, Helvetica Neue, system-ui
- 等宽：JetBrains Mono, Menlo, Consolas

| 用途 | 字号 | 行高 | 字重 |
|------|------|------|------|
| 大标题（少用） | 20 / 24 | 28 / 32 | 500–600 |
| 顶栏系统名 | 16 | 与顶栏对齐 | 600 |
| 区块标题 | 16 | 24 | 500 |
| 正文 | **14** | **22**（约 1.5×） | 400 |
| 辅助 | 13 / 12 | 20 / 18 | 400 |
| 表格 | 14 | 22 | 400 |

## 5. 圆角

| Token | 值 | 用途 |
|-------|-----|------|
| none | 0 | 贴边表格 |
| small | 2px | Tag |
| medium | 4px | 按钮、输入、菜单选中 |
| large | 8px | 卡片、弹窗 |
| circle | 50% | 头像 |

禁止 16–24px 大圆角风。

## 6. 间距（4 的倍数）

4 / 8 / 12 / 16 / 20 / 24 / 32 / 40 / 48

控件间距 8–12；表单项约 20；卡片内 16–24；内容边距 16–24；模块间距 16–20。

## 7. 阴影（海拔）

无/极低（卡片可仅边框）→ 低（卡片轻抬）→ 中（下拉/气泡）→ 高（Modal）。禁止彩色重投影。

## 8. 尺寸阶梯

mini | small | default | large — 密集场景优先 small/default；同排一致。

## 9. 图标尺寸

按钮内 14–16；顶栏/菜单 16–18；宫格 20–22。IconPark outline + currentColor。

## 10. CSS 变量

`css
var(--color-primary-6)
var(--color-text-1)
var(--color-border-2)
var(--color-fill-2)
var(--border-radius-medium)
`

顶栏：ackground: #004EA2;
