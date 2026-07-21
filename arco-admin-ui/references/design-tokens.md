# ui组业务组件 · Design Tokens（默认主题 Light）

实现时优先用 CSS 变量 / less token，勿硬编码随意色值。下列 token 与实现组件库默认主题对齐。

## 0. 产品壳固定色（优先于其它主色讨论）

| 用途 | 色值 | 说明 |
|------|------|------|
| **顶栏 Header 背景** | `#004EA2` | **确定样式**，通栏深蓝，白字 |
| 顶栏文字/图标 | `#FFFFFF` | |
| **菜单选中背景** | `#E6F1FF` | **确定样式** |
| 菜单选中文字/图标 | `#165DFF`（可略亮如 `#398EFF`） | |
| 侧栏背景 | `#FFFFFF` | 非深色侧栏 |
| 内容区背景 | `#F2F3F5` / `#F7F8FA` | |
| 主按钮 primary | `#165DFF` | 查询/添加/处理等 |

顶栏 `#004EA2` 与按钮主色 `#165DFF` **可以不同**；不要把顶栏改成按钮蓝，也不要把按钮改成顶栏深蓝，除非用户明确要求。

## 1. 品牌与语义色

| Token 语义 | 默认值 | 说明 |
|------------|--------|------|
| primary-6（主色） | `#165DFF` | 主按钮、链接、选中文字、焦点 |
| primary-5 / hover | `#4080FF` | 主色悬停（约） |
| primary-7 / active | `#0E42D2` | 主色按下（约） |
| success-6 | `#00B42A` | 成功 |
| warning-6 | `#FF7D00` | 警告 |
| danger-6 | `#F53F3F` | 错误/危险/删除文字 |
| link | 同 primary | 文字链接、表格操作 |

主色阶（primary-1…primary-10 思路）：浅背景用 primary-1 / `#E6F1FF` / `#E8F3FF` 一类浅色，深强调用 primary-7。

## 2. 文字色（四级）

| Token | 默认值 | 用途 |
|-------|--------|------|
| color-text-1 | `#1D2129` | 标题、强调正文 |
| color-text-2 | `#4E5969` | 次级正文 |
| color-text-3 | `#86909C` | 辅助说明、占位 |
| color-text-4 | `#C9CDD4` | 禁用、更弱提示 |

## 3. 填充 / 背景 / 边框

| Token | 默认值 | 用途 |
|-------|--------|------|
| color-bg-1 | `#FFFFFF` | 页面主背景（或纯白内容） |
| color-bg-2 | `#F7F8FA` | 次级背景、表头、侧栏浅底 |
| color-fill-1 | `#F7F8FA` | 浅填充 |
| color-fill-2 | `#F2F3F5` | 默认填充、内容区底 |
| color-fill-3 | `#E5E6EB` | 悬停填充 |
| color-border-1 | `#F2F3F5` | 极浅分割 |
| color-border-2 | `#E5E6EB` | 默认边框、分割线 |
| color-border-3 | `#C9CDD4` | 更强边框 |
| color-border-4 | `#86909C` | 强调边框 |

暗色主题仅在用户明确要求时使用；默认 Light。

## 4. 字体

- 中文：`PingFang SC`, `Microsoft YaHei`, `Hiragino Sans GB`
- 西文/数字：`Inter`, `Helvetica Neue`, system-ui
- 等宽：`JetBrains Mono`, `Menlo`, `Consolas`（代码/ID）

### 字号阶梯（常用）

| 用途 | 字号 | 行高建议 | 字重 |
|------|------|----------|------|
| 页面大标题 | 20 / 24 | 28 / 32 | 500–600 |
| 顶栏系统名 | 16 | 52px 行高 | 600 |
| 区块标题 | 16 / 18 | 24 / 26 | 500 |
| 正文 | 14 | 22 | 400 |
| 辅助 | 13 / 12 | 20 / 18 | 400 |
| 表格默认 | 14 | 22 | 400 |

后台界面默认正文 **14px**，不要用过大展示标题破坏密度。

## 5. 圆角

| Token | 值 | 用途 |
|-------|-----|------|
| border-radius-none | 0 | 贴边、表格 |
| border-radius-small | 2px | 标签、小控件 |
| border-radius-medium | 4px | 按钮、输入框、菜单选中项 |
| border-radius-large | 8px | 卡片、弹窗 |
| border-radius-circle | 50% | 头像 |

**注意**：本规范默认偏「小圆角」，禁止 16–24px 大圆角卡片风。

## 6. 间距（4 的倍数）

常用：`4 / 8 / 12 / 16 / 20 / 24 / 32 / 40 / 48`

| 场景 | 建议 |
|------|------|
| 控件内边距 | 按钮 default 水平 16，small 12 |
| 表单项垂直间距 | 20 或 Form 默认 |
| 卡片内边距 | 16 / 20 / 24 |
| 内容区页边距 | 16 / 20 / 24 |
| 筛选区与表格间距 | 16 / 20 |

## 7. 阴影

- 卡片轻阴影：低海拔，避免重拟态
- 下拉/弹层：中等阴影，层次清晰
- 不要大面积彩色投影

## 8. 尺寸阶梯（组件 size）

统一使用：`mini` | `small` | `default` | `large`

中后台表格/筛选密集场景优先 **`small`** 或 **default**，少用 large。

## 9. CSS 变量使用提示

引入官方样式后，优先：

```css
var(--color-primary-6)
var(--color-text-1)
var(--color-border-2)
var(--color-fill-2)
var(--border-radius-medium)
```

产品壳顶栏请直接使用固定色：

```css
background: #004EA2;
```

主题定制可覆盖按钮 primary，但顶栏与菜单选中态默认按第 0 节，除非用户明确改壳。
