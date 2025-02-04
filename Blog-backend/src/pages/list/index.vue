<script setup>
  import { ref, reactive, onMounted, createVNode } from 'vue';
  import { Modal, message } from 'ant-design-vue';
  import { ExclamationCircleOutlined, PlusOutlined, DeleteOutlined, FileSyncOutlined, LinkOutlined, FireFilled } from '@ant-design/icons-vue';

  import imgbg1 from '@/assets/images/1.png';
  import imgbg2 from '@/assets/images/2.png';

  const formData = reactive({
    categoryId: undefined,
    tagId: undefined,
    articleTitle: undefined,
    articleType: undefined,
    isTop: undefined,
    status: undefined,
  });

  // 分类
  const categoryList = ref([]);
  // 标签
  const tagList = ref([]);

  const loading = ref(false);
  const tabData = ref([]); // 表格数据

  onMounted(async () => {
    await refreshFunc();
    await getCategory();
    await getTag();
  });

  async function getCategory() {
    // 模拟分类数据
    categoryList.value = [
      { id: 1, categoryName: '技术' },
      { id: 2, categoryName: '生活' },
      { id: 3, categoryName: '旅行' },
    ];
  }

  async function getTag() {
    // 模拟标签数据
    tagList.value = [
      { id: 1, tagName: 'Vue' },
      { id: 2, tagName: 'JavaScript' },
      { id: 3, tagName: '旅行' },
    ];
  }

  async function refreshFunc() {
    loading.value = true;
    // 模拟文章数据
    tabData.value = [
      {
        id: 1,
        articleCover: imgbg1,
        articleTitle: 'Vue 3 入门指南',
        articleType: 1,
        categoryName: '技术',
        tagsName: ['Vue', 'JavaScript'],
        status: 1,
        isTop: true,
        visitCount: 100,
        userName: '张三',
        createTime: '2023-10-01 12:00:00',
      },
      {
        id: 2,
        articleCover: imgbg2,
        articleTitle: 'JavaScript 高级技巧',
        articleType: 2,
        categoryName: '技术',
        tagsName: ['JavaScript'],
        status: 2,
        isTop: false,
        visitCount: 200,
        userName: '李四',
        createTime: '2023-10-02 12:00:00',
      },
    ];
    loading.value = false;
  }

  // 选中状态
  const state = reactive({
    selectedRowKeys: [],
    loading: false,
  });

  // 选中
  function onSelectChange(selectedRowKeys) {
    state.selectedRowKeys = selectedRowKeys;
  }

  const columns = [
    {
      title: '编号',
      dataIndex: 'id',
      align: 'center',
    },
    {
      title: '封面',
      dataIndex: 'articleCover',
      align: 'center',
    },
    {
      title: '标题',
      dataIndex: 'articleTitle',
      align: 'center',
    },
    {
      title: '类型',
      dataIndex: 'articleType',
      align: 'center',
    },
    {
      title: '分类',
      dataIndex: 'categoryName',
      align: 'center',
    },
    {
      title: '标签',
      dataIndex: 'tagsName',
      align: 'center',
    },
    {
      title: '状态',
      dataIndex: 'status',
      align: 'center',
    },
    {
      title: '是否顶置',
      dataIndex: 'isTop',
      align: 'center',
    },
    {
      title: '访问量',
      dataIndex: 'visitCount',
      align: 'center',
    },
    {
      title: '作者',
      dataIndex: 'userName',
      align: 'center',
    },
    {
      title: '发布时间',
      dataIndex: 'createTime',
      align: 'center',
    },
    {
      title: '操作',
      dataIndex: 'operation',
      align: 'center',
    },
  ];

  async function onFinish(values) {
    loading.value = true;
    // 模拟搜索功能
    const filteredData = tabData.value.filter((item) => {
      return (
        (!values.articleTitle || item.articleTitle.includes(values.articleTitle)) &&
        (!values.categoryId || item.categoryName === categoryList.value.find(cat => cat.id === values.categoryId)?.categoryName) &&
        (!values.status || item.status === values.status) &&
        (!values.isTop || item.isTop === (values.isTop === 1))
      );
    });

    if (filteredData.length > 0) {
      tabData.value = filteredData;
    } else {
      message.warn('没有查询到相关文章');
      tabData.value = [];
    }
    loading.value = false;
  }

  // 修改状态
  function updateArticleStatusFunc(id, status, record) {
    record.statusLoading = true;
    setTimeout(() => {
      message.success('状态修改成功');
      record.status = status;
      record.statusLoading = false;
    }, 1000);
  }

  // 是否顶置
  function updateIsTopFunc(id, isTop, record) {
    record.isToploading = true;
    setTimeout(() => {
      message.success('操作成功');
      record.isTop = isTop;
      record.isToploading = false;
    }, 1000);
  }

  // 删除文章
  function onDelete(ids) {
    if (ids) {
      tabData.value = tabData.value.filter(item => !ids.includes(item.id));
      message.success('删除成功');
    } else {
      const ids = state.selectedRowKeys;
      Modal.confirm({
        title: '注意',
        icon: createVNode(ExclamationCircleOutlined),
        content: `确定删除编号为 【${ids.join(',')}】 的文章吗？`,
        okText: '确认',
        cancelText: '取消',
        onOk: () => {
          tabData.value = tabData.value.filter(item => !ids.includes(item.id));
          message.success('删除成功');
        },
      });
    }
  }

</script>

<template>
  <div class="list">
    <div class="crumbs">
      <a-breadcrumb>
        <a-breadcrumb-item>网站管理</a-breadcrumb-item>
        <a-breadcrumb-item>文章管理</a-breadcrumb-item>
        <a-breadcrumb-item>文章列表</a-breadcrumb-item>
      </a-breadcrumb>
    </div>
    <layoutTop :form-state="formData" @update:refresh-func="refreshFunc" @update:on-finish="onFinish">
      <template #form-items>
        <a-form-item label="标题" name="articleTitle" style="margin-right: 1rem">
          <a-input v-model:value="formData.articleTitle" placeholder="输入文章标题" style="width: 15em" />
        </a-form-item>
        <a-form-item label="分类" name="categoryId" style="margin-right: 1rem">
          <a-space>
            <a-select v-if="categoryList" v-model:value="formData.categoryId" placeholder="选择分类" style="width: 13em" :options="categoryList.map(item => ({ value: item.id, label: item.categoryName }))" />
          </a-space>
        </a-form-item>
        <a-form-item label="状态" name="status" style="margin-right: 1rem">
          <a-space>
            <a-select v-model:value="formData.status" style="width: 10em" placeholder="选择状态">
              <a-select-option :value="1">
                公开
              </a-select-option>
              <a-select-option :value="2">
                私密
              </a-select-option>
              <a-select-option :value="3">
                草稿
              </a-select-option>
            </a-select>
          </a-space>
        </a-form-item>
        <a-form-item label="是否顶置" name="isTop" style="margin-right: 1rem">
          <a-select v-model:value="formData.isTop" style="width: 13em" placeholder="是否顶置">
            <a-select-option :value="1">
              是
            </a-select-option>
            <a-select-option :value="0">
              否
            </a-select-option>
          </a-select>
        </a-form-item>
      </template>
      <template #operate-btn>
        <a-button type="primary" style="margin-right: 10px" @click="$router.push({ path: '/publish' })">
          <template #icon>
            <PlusOutlined />
          </template>
          添加
        </a-button>
        <a-button type="dashed" danger ghost :disabled="state.selectedRowKeys.length === 0" @click="onDelete()">
          <template #icon>
            <DeleteOutlined />
          </template>
          删除
        </a-button>
        <a-button class="green" style="margin-right: 10px" :disabled="state.selectedRowKeys.length === 0 || state.selectedRowKeys.length > 1" @click="$router.push({ path: '/publish', query: { id: state.selectedRowKeys[0] } })">
          <template #icon>
            <FileSyncOutlined />
          </template>
          修改
        </a-button>
      </template>
      <template #table-content>
        <a-table :columns="columns" :data-source="tabData" :loading="loading" :row-selection="{ selectedRowKeys: state.selectedRowKeys, onChange: onSelectChange }" :row-key="record => record.id" size="small">
          <template #bodyCell="{ column, record }">
            <template v-if="column.dataIndex === 'articleCover'">
              <a-image :src="record.articleCover" style="width: 70px; height: 50px" />
            </template>
            <template v-if="column.dataIndex === 'articleType'">
              <!--            类型 (1原创 2转载 3翻译) -->
              <a-tag color="#108ee9">
                {{ record.articleType === 1 ? '原创' : record.articleType === 2 ? '转载' : '翻译' }}
              </a-tag>
            </template>
            <template v-if="column.dataIndex === 'categoryName'">
              <a-tag>{{ record.categoryName }}</a-tag>
            </template>
            <template v-if="column.dataIndex === 'tagsName'">
              <template v-if="record.tagsName.length <= 2">
                <a-tag v-for="tag in record.tagsName" :key="tag" color="blue">
                  {{ tag }}
                </a-tag>
              </template>
              <template v-else>
                <a-popover trigger="hover">
                  <template #content>
                    <a-tag v-for="tag in record.tagsName" :key="tag" color="blue">
                      {{ tag }}
                    </a-tag>
                  </template>
                  <a-tag v-for="tag in record.tagsName.slice(0, 1)" :key="tag" color="blue">
                    {{ tag }}
                  </a-tag>
                  <a-tag>
                    +{{ record.tagsName.length - 1 }}
                  </a-tag>
                </a-popover>
              </template>
            </template>
            <template v-if="column.dataIndex === 'status'">
              <a-space>
                <a-select v-model:value="record.status" style="width: 6em" size="small" :loading="record.statusLoading" @change="updateArticleStatusFunc(record.id, record.status, record)">
                  <a-select-option :value="1">
                    公开
                  </a-select-option>
                  <a-select-option :value="2">
                    私密
                  </a-select-option>
                  <a-select-option :value="3">
                    草稿
                  </a-select-option>
                </a-select>
              </a-space>
            </template>
            <template v-if="column.dataIndex === 'isTop'">
              <a-switch v-model:checked="record.isTop" checked-children="是" un-checked-children="否" :loading="record.isToploading" @change="updateIsTopFunc(record.id, record.isTop, record)" />
            </template>
            <template v-if="column.dataIndex === 'visitCount'">
              <span style="color: darkred">
                <FireFilled />
              </span>
              {{ record.visitCount }}
            </template>
            <template v-if="column.dataIndex === 'operation'">
              <!-- <a :href="`${domain}article/${record.id}`" target="_blank">
                <a-button type="link" style="padding: 0">
                  <template #icon>
                    <LinkOutlined />
                  </template>
                  <span style="margin-left: 3px">跳转</span>
                </a-button>
              </a> -->
              <a-button type="link" style="padding: 0" @click="$router.push({ path: '/publish', query: { id: record.id } })">
                <template #icon>
                  <FileSyncOutlined />
                </template>
                <span style="margin-left: 3px">修改</span>
              </a-button>
              <a-popconfirm title="是否确定删除" ok-text="Yes" cancel-text="No" @confirm="onDelete([record.id])">
                <a-button type="link" style="padding: 0">
                  <template #icon>
                    <DeleteOutlined />
                  </template>
                  <span style="margin-left: 3px">删除</span>
                </a-button>
              </a-popconfirm>
            </template>
            <template v-if="column.dataIndex === 'createTime'">
              <a-tooltip placement="top">
                <template #title>
                  <span>{{ record.createTime }}</span>
                </template>
                {{ record.createTime.split(' ')[0] }}...
              </a-tooltip>
            </template>
          </template>
        </a-table>
      </template>
    </layoutTop>
  </div>
</template>

<style lang="scss" scoped>
.list{
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
</style>