<script setup>
  import { ref, reactive, onMounted, computed, createVNode, h } from 'vue';
  import { Modal, message } from 'ant-design-vue';
  import {
    ExclamationCircleOutlined,
    PlusOutlined,
    FileSyncOutlined,
    DeleteOutlined,
    VerticalAlignBottomOutlined,
    TagOutlined
  } from '@ant-design/icons-vue';

  import dayjs from 'dayjs';
  import isBetween from 'dayjs/plugin/isBetween'
  dayjs.extend(isBetween);

  // 模拟数据
  const mockData = [
    {
      id: 1,
      tagName: 'Vue',
      articleCount: 10,
      createTime: '2023-10-01 12:00:00',
      updateTime: '2023-10-01 12:00:00',
    },
    {
      id: 2,
      tagName: 'React',
      articleCount: 5,
      createTime: '2023-10-02 14:00:00',
      updateTime: '2023-10-02 14:00:00',
    },
    {
      id: 3,
      tagName: 'Node.js',
      articleCount: 8,
      createTime: '2023-10-03 16:00:00',
      updateTime: '2023-10-03 16:00:00',
    },
  ];

  // 表单数据
  const formState = reactive({
    tagName: undefined,
    time: undefined,
  });

  // 表格列配置
  const columns = [
    {
      title: '标签编号',
      dataIndex: 'id',
      align: 'center',
    },
    {
      title: '标签名称',
      dataIndex: 'tagName',
      align: 'center',
    },
    {
      title: '文章数量',
      dataIndex: 'articleCount',
      align: 'center',
    },
    {
      title: '创建时间',
      dataIndex: 'createTime',
      align: 'center',
    },
    {
      title: '更新时间',
      dataIndex: 'updateTime',
      align: 'center',
    },
    {
      title: '操作',
      dataIndex: 'operation',
      key: 'operation',
      align: 'center',
    },
  ];

  // 加载状态
  const loading = ref(false);

  // 表格数据
  const tabData = ref([]);

  // 初始化加载数据
  onMounted(() => {
    refreshFunc();
  });

  // 刷新数据
  async function refreshFunc(searchData) {
    loading.value = true;
    // 模拟异步请求
    setTimeout(() => {
      tabData.value = searchData || mockData;
      loading.value = false;
    }, 500);
  }

  // 查询标签
  async function onFinish(values) {
    loading.value = true;
    // 模拟查询逻辑
    const filteredData = mockData.filter((item) => {
      const matchesTagName = values.tagName ? item.tagName.includes(values.tagName) : true;
      const matchesTime = values.time && values.time[0] && values.time[1]
        ? dayjs(item.createTime).isBetween(values.time[0], values.time[1])
        : true;
      return matchesTagName && matchesTime;
    });
    await refreshFunc(filteredData);
  }

  // 表格状态
  const state = reactive({
    selectedRowKeys: [],
    loading: false,
  });

  // 选中行变化
  function onSelectChange(selectedRowKeys) {
    state.selectedRowKeys = selectedRowKeys;
  }

  const hasSelected = computed(() => state.selectedRowKeys.length === 1);

  // 模态框信息
  const modalInfo = reactive({
    open: false,
    title: '',
    loading: false,
  });

  // 表单数据
  const formData = ref({});

  // 删除标签
  function deleteTag(ids, type = 0) {
    if (type === 0) {
      Modal.confirm({
        title: '注意',
        icon: h(ExclamationCircleOutlined),
        content: `确定删除编号为 【${ids.join(',')}】 的标签吗？`,
        okText: '确认',
        cancelText: '取消',
        onOk: () => {
          tabData.value = tabData.value.filter((item) => !ids.includes(item.id));
          message.success('删除成功');
          state.selectedRowKeys = [];
        },
      });
    } else {
      tabData.value = tabData.value.filter((item) => !ids.includes(item.id));
      message.success('删除成功');
      state.selectedRowKeys = [];
    }
  }

  // 添加或修改标签
  function updateOrInsertTag(id) {
    if (id) {
      const tagInfo = tabData.value.find((item) => item.id === id);
      formData.value = { ...tagInfo };
      modalInfo.title = '修改标签';
    } else {
      formData.value = {};
      modalInfo.title = '添加标签';
    }
    modalInfo.open = true;
  }

  // 模态框确认
  async function modelOk() {
    modalInfo.loading = true;
    if (formData.value.id) {
      // 模拟修改逻辑
      const index = tabData.value.findIndex((item) => item.id === formData.value.id);
      if (index !== -1) {
        tabData.value[index] = { ...formData.value };
        message.success('修改成功');
      }
    } else {
      // 模拟添加逻辑
      const newTag = {
        id: tabData.value.length + 1,
        ...formData.value,
        createTime: new Date().toISOString(),
        updateTime: new Date().toISOString(),
      };
      tabData.value.push(newTag);
      message.success('添加成功');
    }
    modalInfo.loading = false;
    modalInfo.open = false;
    await refreshFunc();
  }
</script>

<template>
  <div class="tag">
    <div class="crumbs">
      <a-breadcrumb>
        <a-breadcrumb-item>网站管理</a-breadcrumb-item>
        <a-breadcrumb-item>标签管理</a-breadcrumb-item>
      </a-breadcrumb>
    </div>
    <layoutTop :form-state="formState" @update:refresh-func="refreshFunc" @update:on-finish="onFinish">
      <template #form-items>
        <a-form-item label="标签名称" name="tagName">
          <a-input v-model:value="formState.tagName" placeholder="请输入标签名称" style="width: 250px" />
        </a-form-item>
        <a-form-item label="创建时间" name="time">
          <a-range-picker v-model:value="formState.time" :placeholder="['开始时间', '结束时间']" />
        </a-form-item>
      </template>

      <template #operate-btn>
        <a-button type="primary" @click="updateOrInsertTag()">
          <template #icon>
            <PlusOutlined />
          </template>
          新增
        </a-button>
        <a-button class="green" :disabled="!hasSelected" @click="updateOrInsertTag(state.selectedRowKeys[0])">
          <template #icon>
            <FileSyncOutlined />
          </template>
          修改
        </a-button>
        <a-button type="dashed" danger ghost :disabled=" !(state.selectedRowKeys.length > 0) " @click="deleteTag(state.selectedRowKeys, 0)">
          <template #icon>
            <DeleteOutlined />
          </template>
          删除
        </a-button>
      </template>

      <template #table-content>
        <a-modal v-model:open="modalInfo.open" :title="modalInfo.title" :confirm-loading="modalInfo.loading" width="400px" @ok="modelOk">
          <a-form-item label="标签名称" name="tagName">
            <a-input v-model:value="formData.tagName" placeholder="请输入标签名称" show-count :maxlength="20" />
          </a-form-item>
        </a-modal>
        <a-table :columns="columns" :data-source="tabData" :loading="loading" :row-selection="{ selectedRowKeys: state.selectedRowKeys, onChange: onSelectChange }" :row-key="record => record.id" size="small">
          <template #bodyCell="{ column, record }">
            <template v-if="column.dataIndex === 'tagName'">
              <a-tag color="blue">
                <TagOutlined /><span>{{ record.tagName }}</span>
              </a-tag>
            </template>
            <template v-if="column.dataIndex === 'articleCount'">
              <a-tag>
                {{ record.articleCount }}
              </a-tag>
            </template>
            <template v-if="column.key === 'operation'">
              <a-button type="link" style="padding: 0;" @click="updateOrInsertTag(record.id)">
                <template #icon>
                  <FileSyncOutlined />
                </template>
                <span style="margin-inline-start:1px">修改</span>
              </a-button>
              <a-popconfirm title="是否确定删除" ok-text="Yes" cancel-text="No" @confirm="deleteTag([record.id])">
                <a-button type="link" style="padding: 0;margin-left: 5px">
                  <template #icon>
                    <DeleteOutlined />
                  </template>
                  <span style="margin-inline-start:1px">删除</span>
                </a-button>
              </a-popconfirm>
            </template>
            <template v-else-if="column.key === 'icon'">
              <!-- 图标 -->
              <component :is="record.icon" />
            </template>
          </template>
        </a-table>
      </template>
    </layoutTop>
  </div>
</template>

<style lang="scss" scoped>
.tag{
  background: white;
  min-height: 50vh;
  margin-bottom: 5rem;
  .crumbs {
    padding: 1rem;
    h1{
      margin-top: 1rem;
      font-size: 1.2rem;
      font-weight: 500;
    }
  }
}

:deep(.ant-select-arrow){
  padding-top: 0.8rem;
}

:deep(.ant-picker-suffix){
  padding-top: 0.45rem;
}
</style>