# 模板 · 工作台（最小骨架）

> 套用前先写 page-spec。文案/指标名按业务替换。工作台**不要**内容区大标题。

```tsx
import React, { useEffect, useState } from 'react';
import { Layout, Menu, Card, Grid, Table, Button, Spin, Message } from '@/components';
// '@/components' = 项目实现层组件库入口

const { Header, Sider, Content } = Layout;
const { Row, Col } = Grid;

// services/workbench.ts 占位
async function fetchWorkbench() {
  // TODO: GET /api/workbench/summary
  return {
    todos: [{ id: '1', title: '待办示例', status: '今日' }],
    metrics: [{ label: '完成率', value: 72 }],
    recent: [{ id: '1', name: '示例任务', owner: '张三', status: '进行中' }],
  };
}

// 侧栏折叠状态（壳层必有）
// const [collapsed, setCollapsed] = useState(false);
export default function WorkbenchPage() {
  const [loading, setLoading] = useState(true);
  const [data, setData] = useState<any>(null);

  useEffect(() => {
    (async () => {
      try {
        setLoading(true);
        setData(await fetchWorkbench());
      } catch {
        Message.error('加载失败'); // 文案可配置
      } finally {
        setLoading(false);
      }
    })();
  }, []);

  return (
    <Layout style={{ height: '100vh' }}>
      <Header style={{ background: '#004EA2', color: '#fff' }}>
        {/* 系统名按业务 */}
        <span>业务系统</span>
        {/* 右侧：主题切换、用户 */}
      </Header>
      <Layout>
        <Sider theme="light" collapsible collapsedWidth={48} collapsible collapsedWidth={48} width={220} style={{ background: '#fff' }}>
          <Menu selectedKeys={['workbench']} style={{ background: '#fff' }}>
            <Menu.Item key="workbench">工作台</Menu.Item>
            {/* 选中态：浅蓝底 #E6F1FF + 主色字，按实现层 Menu 样式/覆盖 */}
          </Menu>
        </Sider>
        <Content style={{ padding: 16, background: 'var(--color-fill-2)' }}>
          {loading ? (
            <Spin />
          ) : (
            <Row gutter={16}>
              <Col flex="1">
                <Card title="我的待办" style={{ marginBottom: 16 }}>
                  {/* 待办列表；无数据 Empty */}
                </Card>
                <Card title="近期任务">
                  <Table
                    size="small"
                    border
                    data={data?.recent || []}
                    columns={[
                      { title: '名称', dataIndex: 'name' },
                      { title: '负责人', dataIndex: 'owner' },
                      { title: '状态', dataIndex: 'status' },
                    ]}
                    pagination={false}
                  />
                </Card>
              </Col>
              <Col flex="320px">
                <Card title="快捷入口" style={{ marginBottom: 16 }}>
                  {/* 宫格功能入口 IconPark */}
                </Card>
                <Card title="汇总指标">
                  {/* Statistic / Progress 线形，见 progress-patterns */}
                </Card>
              </Col>
            </Row>
          )}
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

