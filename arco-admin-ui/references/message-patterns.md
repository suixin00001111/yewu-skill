# ui组业务组件 · 全局提示 Message

> 全局轻量反馈：成功、警告、失败、说明等，**不打断**用户当前操作。  
> 实现对齐实现层 `Message` 组件能力；界面文案不出现第三方库品牌名。  
> 删除完成、保存成功、提交失败等**结果类**反馈必须走 Message，不要再弹一层「成功 Modal」。

---

## 1. 何时用 Message

| 场景 | 是否用 Message | 说明 |
|------|----------------|------|
| 删除确认后成功 | **必须** | 先 Modal/Popconfirm 确认 → 接口成功 → `Message.success('删除成功')` |
| 保存/提交成功 | **必须** | 如「保存成功」「提交成功」 |
| 操作失败/校验失败（接口） | **必须** | `Message.error` / 关键错误用 error |
| 表单字段校验 | 否 | 用 Form 字段错误，不整页 Message 刷屏 |
| 需要用户二次决策 | 否 | 用 Modal / Popconfirm |
| 长文说明、多操作入口 | 否 | 用 Notification 或页内 Alert |
| 仅页内常驻提示 | 否 | 用 Alert |

### 删除完整链路（强制）

```
1. 用户点「删除」
2. Popconfirm 或 Modal 确认（类型 A）
3. 请求中：可选 Message.loading / 按钮 loading
4. 成功：Message.success（如「删除成功」）
5. 失败：Message.error（如「删除失败，请重试」）
```

**禁止**：确认后只改表格、无任何全局提示；禁止成功后再弹「操作成功」对话框。

---

## 2. 类型（5 + loading）

| 类型 | API 习惯 | 图标语义色 | 典型文案 |
|------|----------|------------|----------|
| **成功** | `success` | `#00B42A` | 删除成功、保存成功、提交成功、复制成功 |
| **失败** | `error` | `#F53F3F` | 删除失败、保存失败、网络异常 |
| **警示** | `warning` | `#FF7D00` | 部分成功、即将逾期、权限不足提示 |
| **指南/信息** | `info` | `#165DFF` | 说明性提示、已切换环境 |
| **普通** | `normal` | 无图标或中性 | 纯文案轻提示 |
| **加载** | `loading` | 主色 | 提交中…、导出中…（可随后 update 为 success/error） |

按**通知等级**选类型，不要一律 success。

---

## 3. 外观与位置

| 项 | 规范 |
|----|------|
| 位置 | 默认 **顶部居中**（距顶约 40px）；需要时可用底部，整站保持一致 |
| 形态 | 白/浮层底 + 浅中性描边 + 轻阴影；圆角小（约 2–4） |
| 内容 | **左图标 + 右文案**（normal 可无图标）；正文 14px / `color-text-1` |
| 内边距 | 约 10×16；多条纵向堆叠，间距约 16 |
| 关闭 | 默认自动消失；重要提示可 `closable` 手动关 |
| 时长 | 默认约 **3s**；错误可略长（4–5s）；loading 不自动关直到结束 |
| 数量 | 控制最大同时条数，避免刷屏；同类短时可合并/更新同一 id |
| z-index | 高于页面内容与表格，低于或按实现层与 Modal 约定（结果提示仍用 Message，不用 Modal） |

### 文案原则

- 清晰描述**结果**：优先「删除成功」而非「操作完成」含糊语  
- 失败时尽量说明原因或下一步：「删除失败，请稍后重试」  
- 一句为主，不写长段落；不写规范注释/实现说明进 UI  
- 中文业务话术；与按钮动词一致（删除→删除成功）

---

## 4. 代码骨架（React 示例）

```tsx
import { Message, Modal } from '@/components'; // 实现层组件库入口（以项目为准）

// 删除
function onDelete(row) {
  Modal.confirm({
    title: '提示',
    content: '确认删除该条数据？',
    okText: '确认',
    cancelText: '取消',
    onOk: async () => {
      try {
        await api.remove(row.id);
        Message.success('删除成功');
        refreshList();
      } catch (e) {
        Message.error('删除失败，请重试');
        throw e;
      }
    },
  });
}

// 保存
async function onSave() {
  try {
    await api.save(form);
    Message.success('保存成功');
  } catch {
    Message.error('保存失败');
  }
}

// 提交中
const close = Message.loading({ content: '提交中...', duration: 0 });
try {
  await api.submit();
  close();
  Message.success('提交成功');
} catch {
  close();
  Message.error('提交失败');
}
```

Vue 侧使用实现层等价 API（`Message.success` 等），逻辑相同。

---

## 5. 与其它反馈组件分工

| 组件 | 角色 |
|------|------|
| **Message** | 操作结果的全局轻提示（成功/失败/警示/加载） |
| **Notification** | 更长文案、可带标题/操作、角边弹出 |
| **Modal / Popconfirm** | 决策前确认（删除前） |
| **Alert** | 页内常驻说明 |
| **Form 校验** | 字段级错误 |

---

## 6. 生成自检

- [ ] 删除/保存/提交成功出现全局 success 提示
- [ ] 失败有 error 提示，不只 console
- [ ] 删除前有确认，成功后是 Message 不是成功 Modal
- [ ] 默认顶部居中；有图标语义色
- [ ] 自动关闭约 3s；不打断表单填写焦点（除非 error 需关注）
- [ ] 界面无第三方库品牌名、无 skill 注释文案
