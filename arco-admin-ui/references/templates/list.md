# 模板 · 列表页（最小骨架）

> 页名单独一行；新建在表格上方工具行右侧。文案可配置。

```tsx
import React, { useEffect, useState } from 'react';
import {
  Layout, Menu, Card, Form, Input, Select, Button, Table, Pagination,
  Empty, Spin, Message, Modal, Space,
} from '@/components';

// services/list.ts 占位
async function fetchList(query: any) {
  // TODO: GET /api/items
  return { list: [] as any[], total: 0 };
}
async function removeItem(id: string) {
  // TODO: DELETE /api/items/:id
}

const { Header, Sider, Content } = Layout;

export default function ListPage() {
  const [form] = Form.useForm();
  const [loading, setLoading] = useState(false);
  const [data, setData] = useState<any[]>([]);
  const [total, setTotal] = useState(0);
  const [page, setPage] = useState(1);
  const [selected, setSelected] = useState<(string | number)[]>([]);

  const load = async (p = page) => {
    setLoading(true);
    try {
      const q = { ...form.getFieldsValue(), page: p };
      const res = await fetchList(q);
      setData(res.list);
      setTotal(res.total);
    } catch {
      Message.error('加载失败');
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => { load(1); }, []);

  const onDelete = (id: string) => {
    Modal.confirm({
      title: '提示', // 可配置
      content: '确认删除该条数据？', // 可配置
      okText: '确认',
      cancelText: '取消',
      onOk: async () => {
        await removeItem(id);
        Message.success('删除成功'); // 可配置
        load();
      },
    });
  };

  return (
    <Layout style={{ height: '100vh' }}>
      <Header style={{ background: '#004EA2', color: '#fff' }}>业务系统</Header>
      <Layout>
        <Sider theme="light" collapsible collapsedWidth={48} width={220}><Menu selectedKeys={['list']} /></Sider>
        <Content style={{ padding: 16, background: 'var(--color-fill-2)' }}>
          <div style={{ color: 'var(--color-text-3)', marginBottom: 12 }}>模块 / 列表</div>
          <Card bodyStyle={{ padding: 20 }}>
            {/* 1. 标题单独一行 */}
            <h1 style={{ margin: '0 0 16px', fontSize: 16, fontWeight: 500 }}>业务列表</h1>

            {/* 2. 筛选 */}
            <Form form={form} layout="inline" style={{ marginBottom: 16 }}>
              <Form.Item field="name" label="名称"><Input placeholder="请输入" allowClear /></Form.Item>
              <Form.Item>
                <Space>
                  <Button type="primary" onClick={() => load(1)}>查询</Button>
                  <Button onClick={() => { form.resetFields(); load(1); }}>重置</Button>
                </Space>
              </Form.Item>
            </Form>

            {/* 3. 工具行：左批量 右新建 */}
            <div style={{ display: 'flex', justifyContent: 'space-between', marginBottom: 12 }}>
              <Space>
                <span>已选 {selected.length} 项</span>
                <Button disabled={!selected.length}>批量删除</Button>
              </Space>
              <Button type="primary">新建</Button>
            </div>

            <Spin loading={loading}>
              {data.length === 0 && !loading ? (
                <Empty description="暂无数据">
                  <Button type="primary">新建</Button>
                </Empty>
              ) : (
                <Table
                  rowKey="id"
                  size="small"
                  border
                  data={data}
                  rowSelection={{ selectedRowKeys: selected, onChange: setSelected }}
                  columns={[
                    { title: '名称', dataIndex: 'name' },
                    {
                      title: '操作',
                      render: (_, row) => (
                        <Button type="text" status="danger" onClick={() => onDelete(row.id)}>删除</Button>
                      ),
                    },
                  ]}
                  pagination={false}
                />
              )}
            </Spin>
            <div style={{ marginTop: 12, textAlign: 'right' }}>
              <Pagination total={total} current={page} onChange={(p) => { setPage(p); load(p); }} />
            </div>
          </Card>
        </Content>
      </Layout>
    </Layout>
  );
}
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

