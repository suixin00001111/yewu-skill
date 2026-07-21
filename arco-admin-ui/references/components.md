# ui组业务组件 · 组件选型与用法

全局壳（顶栏/菜单）必须先符合 [product-shell.md](product-shell.md)。

实现组件库：`@arco-design/web-react`（默认）/ `@arco-design/web-vue`（包名勿改，保证可运行）。

## 1. 页面级组件

| 需求 | 组件 | 要点 |
|------|------|------|
| 整体框架 | `Layout` `Layout.Header` `Layout.Sider` `Layout.Content` `Layout.Footer` | 侧栏可折叠；Header 高度约 48–60 |
| 导航菜单 | `Menu` | 支持水平/垂直；选中态用主色 |
| 面包屑 | `Breadcrumb` | 内容区顶部，二级以上页面必加 |
| 页头 | 自定义 PageHeader 区 或 Typography + Space | 左标题右主操作 |
| 栅格 | `Grid` `Row` `Col` | 筛选表单、统计卡片 |
| 间距 | `Space` `Divider` | 按钮组、工具条 |

## 2. 数据录入

| 需求 | 组件 |
|------|------|
| 表单容器 | `Form` `Form.Item` |
| 文本 | `Input` `Input.TextArea` `Input.Search` `Input.Password` |
| 数字 | `InputNumber` |
| 选择 | `Select` `Cascader` `TreeSelect` |
| 日期时间 | `DatePicker` `TimePicker` `RangePicker` |
| 开关/勾选 | `Switch` `Checkbox` `Radio` |
| 上传 | `Upload` |
| 穿梭/评分等 | `Transfer` `Rate` `Slider` |

### Form 约定

- 标签布局：筛选区常用 `layout="inline"`；新建/编辑页用 `layout="vertical"` 或 `horizontal`（label 固定宽度）
- 校验：必填、格式错误文案简洁中文
- 操作区：主按钮 `type="primary"`，次按钮 default，危险 `status="danger"` 或 `Button` danger
- 提交按钮防重复：loading

## 3. 数据展示

| 需求 | 组件 |
|------|------|
| 表格 | `Table` |
| 分页 | `Pagination`（可挂 Table） |
| 描述列表 | `Descriptions` |
| 统计数值 | `Statistic` |
| 标签 | `Tag` |
| 头像/徽章 | `Avatar` `Badge` |
| 树 | `Tree` |
| 时间线/步骤 | `Timeline` `Steps` |
| 空状态 | `Empty` |
| 结果页 | `Result` |
| 骨架屏 | `Skeleton` |
| 折叠/卡片 | `Collapse` `Card` |
| 选项卡 | `Tabs` |
| 图片 | `Image` |

### Table 约定（中后台核心）

- 列：左关键信息，右操作列固定 `fixed="right"`
- 操作列：链接按钮或 `Button type="text"`，危险操作二次确认
- 批量：行选择 `rowSelection` + 顶部批量操作条
- 密度：`size="small"` 或默认；长表格配合滚动 `scroll={{ x, y }}`
- 工具栏：左标题/统计，右「刷新 / denseness / 列设置 / 主操作」
- 筛选：表格上方独立筛选卡片或工具条，查询 + 重置

## 4. 反馈

| 需求 | 组件 |
|------|------|
| 轻提示 | `Message` |
| 通知 | `Notification` |
| 对话框 | `Modal` |
| 抽屉 | `Drawer`（复杂表单、详情） |
| 气泡确认 | `Popconfirm` |
| 全局加载 | `Spin` |

### 反馈约定

- 删除：`Popconfirm` 或 `Modal.confirm`
- 成功/失败：`Message.success` / `Message.error`
- 复杂创建：整页表单或 `Drawer`，勿挤在过小 Modal

## 5. 导航与其它

`Dropdown` `Pagination` `Steps` `Anchor` `BackTop` `Affix` `Progress` `Alert`

## 6. 按钮层级（必须遵守）

| 层级 | 用法 | API |
|------|------|-----|
| 主操作 | 页面唯一主 CTA（新建、提交） | `type="primary"` |
| 次操作 | 取消、导出、重置 | default |
| 文本操作 | 表格行内 | `type="text"` |
| 虚线 | 添加一组 | `type="dashed"` |
| 危险 | 删除 | `status="danger"` 或 danger |

同一视觉焦点区 **主按钮不超过 1 个**。

## 7. 图标

```tsx
import { IconPlus, IconSearch, IconRefresh, IconSettings } from '@arco-design/web-react/icon';
```

图标与文字按钮间距用 `Space` 或按钮内置 icon 位。

## 8. 最小页面骨架示例（React）

```tsx
import { Layout, Menu, Breadcrumb, Button, Table, Card, Form, Input, Space } from '@arco-design/web-react';
import { IconPlus } from '@arco-design/web-react/icon';
import '@arco-design/web-react/dist/css/arco.css';

const { Sider, Header, Content } = Layout;

export function AdminPage() {
  return (
    <Layout style={{ height: '100vh' }}>
      <Sider collapsible>
        <div style={{ height: 48, margin: 12, background: 'var(--color-fill-2)' }} />
        <Menu defaultSelectedKeys={['list']} style={{ width: '100%' }}>
          <Menu.Item key="list">数据列表</Menu.Item>
          <Menu.Item key="settings">设置</Menu.Item>
        </Menu>
      </Sider>
      <Layout>
        <Header style={{ background: 'var(--color-bg-2)', borderBottom: '1px solid var(--color-border-2)' }} />
        <Content style={{ padding: 20, background: 'var(--color-fill-2)' }}>
          <Breadcrumb style={{ marginBottom: 16 }}>
            <Breadcrumb.Item>首页</Breadcrumb.Item>
            <Breadcrumb.Item>数据列表</Breadcrumb.Item>
          </Breadcrumb>
          <Card bordered={false}>
            <Form layout="inline" style={{ marginBottom: 16 }}>
              <Form.Item label="名称" field="name">
                <Input placeholder="请输入" allowClear />
              </Form.Item>
              <Form.Item>
                <Space>
                  <Button type="primary">查询</Button>
                  <Button>重置</Button>
                </Space>
              </Form.Item>
            </Form>
            <div style={{ marginBottom: 16, display: 'flex', justifyContent: 'flex-end' }}>
              <Button type="primary" icon={<IconPlus />}>新建</Button>
            </div>
            <Table columns={[]} data={[]} pagination={{ pageSize: 10 }} />
          </Card>
        </Content>
      </Layout>
    </Layout>
  );
}
```

## 9. 典型页面类型 → 组件组合

| 页面 | 组合 |
|------|------|
| 列表页 | 筛选 Form + 工具栏 + Table + Pagination |
| 新建/编辑 | Form（vertical）+ 底部提交栏；复杂用分步 Steps |
| 详情 | Descriptions + Tabs + 关联 Table |
| 仪表盘 | Row/Col + Card + Statistic + 图表区 + 快捷入口 |
| 登录 | 居中 Card + Form；无侧栏 |
| 设置 | Tabs 或 左侧锚点 + Form |

## 10. 详情页组件组合（参考 detail 截图）

| 区域 | 组件 | 要点 |
|------|------|------|
| 页头 | `Typography.Title` + `Button` | 左标题右「返回」 |
| 流程步骤 | 自定义箭头条 或 `Steps` | 仅多阶段；当前 `#165DFF`，未完成灰 |
| 分块标题 | 自定义蓝竖条 + 文案 | 竖条宽约 3–4px，色 `#165DFF` |
| 字段展示 | `Descriptions` `border` `column={3}` | 标签浅灰底、值白底；长文本 `span` 整行 |
| 明细表 | `Table` `size="small"` | 嵌在分块下方 |
| 空/载 | `Spin` / `Empty` | |

不要用大段纯 `<p>` 堆字段；优先 Descriptions 表格式。
