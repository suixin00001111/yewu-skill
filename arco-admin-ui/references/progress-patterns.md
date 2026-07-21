# ui组业务组件 · 进度条 Progress

> 组件视觉与 API **对齐实现层 Progress**。  
> 界面文案不出现第三方库品牌名。壳层色仍以 product-shell 为准。

---

## 1. 用途

- 表达**任务/流程当前完成度**，减轻等待焦虑。
- 与 **Steps（步骤条）** 区分：
  - **Progress**：百分比/连续进度、上传、导入、指标完成率
  - **Steps**：离散业务阶段（基础信息 → 填写 → 复核）

---

## 2. 构成（必有）

| 部分 | 说明 |
|------|------|
| **当前进度** | 已完成轨道；颜色与剩余轨道对比强烈（默认主色 `#165DFF` / primary-6） |
| **总进度（轨道）** | 未完成部分；**中性色**，无强烈色彩倾向（`color-fill-3` 一类） |
| **进度信息（可选）** | 百分比或自定义文案；默认展示，`showText=false` 可隐藏 |

---

## 3. 类型与状态

| type / 形态 | 场景 |
|-------------|------|
| `line`（默认） | 表单、列表指标、上传、页面加载进度 |
| `circle` | 仪表盘卡片、空间占用、头像旁完成度 |
| `steps`（分段） | 离散分段完成（非业务流程 Steps 组件） |

| status | 色 | 场景 |
|--------|-----|------|
| `normal` | 主色蓝 | 进行中 |
| `success` | `#00B42A` | 已完成 |
| `warning` | `#FF7D00` | 接近上限/风险 |
| `error` | `#F53F3F` | 失败/中断 |
| `color` 自定义 | 优先于 status | 业务特殊色，少用 |

---

## 4. 尺寸与样式习惯

| 项 | 规范 |
|----|------|
| size | `small` / `default` / `mini` / `large`；中后台表格/卡片内优先 **small/default** |
| 线形高度 | 细条约 **4–8px**；卡片强调可用 default |
| 圆角 | 线形轨道与内条 **全圆角**（胶囊） |
| 文案 | 12px，色 `color-text-2`；默认在右侧，`formatText` 可自定义 |
| 动画 | 加载中可用 `animation`；勿全局过度动效 |
| 宽度 | 线形默认铺满容器 `width: 100%`；勿挤成左侧一小截 |

### 推荐 props（React）

```tsx
import { Progress } from '@/components'; // 实现层组件库入口（以项目为准）

// 指标完成率
<Progress percent={72} size="small" />

// 成功
<Progress percent={100} status="success" />

// 失败
<Progress percent={40} status="error" />

// 环形
<Progress type="circle" percent={80} width={80} />

// 无文本
<Progress percent={30} showText={false} />
```

Vue：使用实现层 Vue 组件库等价 `Progress` API。

---

## 5. 业务用法

| 场景 | 建议 |
|------|------|
| 工作台指标卡 | line + small，主色；旁附数值 |
| 详情「完成进度」 | line 全宽，保留模块内边距 |
| 批量导入/上传 | line + loading 动画；结束切 success/error |
| 多阶段审批 | 用 **Steps**，不要用 Progress 冒充步骤 |

---

## 6. 自检

- [ ] 当前进度与轨道对比清晰
- [ ] 轨道中性色，非高饱和整条彩条
- [ ] status 语义正确（完成 success、失败 error）
- [ ] 与 Steps 不混用
- [ ] 使用真实 Progress 组件，不手绘假进度条
- [ ] 界面无库品牌名
