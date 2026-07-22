# 模板 · 详情/编辑页（最小骨架）

> 页头条：左标题 + 右返回/取消/保存。分模块 Card。文案可配置。

```tsx
import React, { useEffect, useState } from 'react';
import {
  Layout, Menu, Card, Button, Space, Descriptions, Table, Steps, Spin, Message,
} from '@/components';

async function fetchDetail(id: string) {
  // TODO: GET /api/items/:id
  return { name: '示例详情', step: 2, rows: [] as any[] };
}
async function saveDetail(id: string, body: any) {
  // TODO: PUT /api/items/:id
}

const { Header, Sider, Content } = Layout;

export default function DetailPage({ id }: { id: string }) {
  const [loading, setLoading] = useState(true);
  const [saving, setSaving] = useState(false);
  const [detail, setDetail] = useState<any>(null);

  useEffect(() => {
    (async () => {
      try {
        setLoading(true);
        setDetail(await fetchDetail(id));
      } catch {
        Message.error('加载失败');
      } finally {
        setLoading(false);
      }
    })();
  }, [id]);

  const onSave = async () => {
    setSaving(true);
    try {
      await saveDetail(id, detail);
      Message.success('保存成功'); // 可配置
    } catch {
      Message.error('保存失败');
    } finally {
      setSaving(false);
    }
  };

  return (
    <Layout style={{ height: '100vh' }}>
      <Header style={{ background: '#004EA2', color: '#fff' }}>业务系统</Header>
      <Layout>
        <Sider theme="light" collapsible collapsedWidth={48} width={220}><Menu selectedKeys={['list']} /></Sider>
        <Content style={{ padding: 16, background: 'var(--color-fill-2)' }}>
          {loading ? <Spin /> : (
            <>
              {/* 页头条 */}
              <div style={{
                background: 'var(--color-bg-2)', border: '1px solid var(--color-border-2)',
                borderRadius: 4, padding: '12px 20px', marginBottom: 16,
                display: 'flex', justifyContent: 'space-between', alignItems: 'center',
              }}>
                <h1 style={{ margin: 0, fontSize: 16, fontWeight: 600 }}>{detail?.name}</h1>
                <Space>
                  <Button>返回</Button>
                  <Button>取消</Button>
                  <Button type="primary" loading={saving} onClick={onSave}>保存</Button>
                </Space>
              </div>

              {/* 可选 Steps：满宽，模块内边距 16–20 */}
              <Card style={{ marginBottom: 16 }} bodyStyle={{ padding: 20 }}>
                <Steps current={detail?.step ?? 1} style={{ width: '100%' }}>
                  <Steps.Step title="基础信息" />
                  <Steps.Step title="明细填写" />
                  <Steps.Step title="确认提交" />
                </Steps>
              </Card>

              <Card title="基础信息" style={{ marginBottom: 16 }} bodyStyle={{ padding: 20 }}>
                <Descriptions
                  column={3}
                  data={[
                    { label: '名称', value: detail?.name },
                    { label: '状态', value: '进行中' },
                  ]}
                />
              </Card>

              <Card title="明细" bodyStyle={{ padding: 20 }}>
                <Table
                  size="small"
                  border
                  borderCell
                  data={detail?.rows || []}
                  columns={[
                    { title: '指标', dataIndex: 'name' },
                    { title: '分值', dataIndex: 'score' },
                  ]}
                  pagination={false}
                />
              </Card>
            </>
          )}
        </Content>
      </Layout>
    </Layout>
  );
}
```

弹窗顶栏背景（若页内有 Modal）：

```css
.modal-header { background: linear-gradient(180deg, #eaf0ff 0%, #ffffff 34%); }
```

>

### 壳层侧栏（强制可折叠）

```tsx
const [collapsed, setCollapsed] = useState(false);
// ...
<Layout hasSider style={{ flex: 1, minHeight: 0 }}>
  <Sider
    theme="light"
    collapsible
    collapsed={collapsed}
    onCollapse={setCollapsed}
    width={220}
    collapsedWidth={48}
    style={{ background: '#fff', borderRight: '1px solid var(--color-border-2)', overflow: 'hidden' }}
  >
    <Menu collapse={collapsed} selectedKeys={[/* ... */]} style={{ height: '100%', borderRight: 0 }}>
      <Menu.Item key="..." icon={/* IconPark */}>菜单名</Menu.Item>
    </Menu>
  </Sider>
  <Content style={{ flex: 1, minWidth: 0, overflow: 'auto', padding: 16, background: 'var(--color-fill-2)' }}>
    {/* 页面内容 */}
  </Content>
</Layout>
```

规则见 [../sider-collapse.md](../sider-collapse.md)：禁止中间灰轨、双 trigger、Sider 外滚动。

