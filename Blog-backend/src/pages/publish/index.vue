<script setup>
  import 'md-editor-v3/lib/style.css'
  import { MdEditor } from 'md-editor-v3'
  import { message } from 'ant-design-vue'
  import { PictureOutlined, PlusOutlined, CloseOutlined } from '@ant-design/icons-vue';
  import { ref, onMounted, defineComponent } from 'vue'
  import { useRoute } from 'vue-router'
  import { useMultiTab } from '../../store/multi-tab'

  const route = useRoute()
  const multiTab = useMultiTab()
  const mode = ref('light')

  const formData = ref({
    categoryId: undefined,
    tagId: undefined,
    articleCover: undefined,
    articleTitle: undefined,
    articleContent: undefined,
    articleType: 1,
    isTop: 0,
    status: 1,
  })

  const VNodes = defineComponent({
    props: {
      vnodes: {
        type: Object,
        required: true,
      },
    },
    render() {
      return this.vnodes
    },
  })

  // 模拟分类数据
  const categoryList = ref([
    { id: 1, categoryName: '技术' },
    { id: 2, categoryName: '生活' },
    { id: 3, categoryName: '旅行' },
  ]);
  // 模拟标签数据
  const tagList = ref([
    { id: 1, tagName: 'Vue' },
    { id: 2, tagName: 'React' },
    { id: 3, tagName: 'Node.js' },
  ]);

  // 文件列表
  const fileList = ref([]);

  // 封面图片的 Base64 编码
  const previewBase64 = ref('');

  // 分类名称输入框的值
  const categoryName = ref('');

  // 标签名称输入框的值
  const tagName = ref('');

  // 分类加载状态
  const categoryLoading = ref(false);

  // 标签加载状态
  const tagLoading = ref(false);


  function getBase64(img) {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.addEventListener('load', () => resolve(reader.result));
      reader.addEventListener('error', (error) => reject(error));
      reader.readAsDataURL(img);
    });
  }

  // 添加分类
  function addCategoryFunc(e) {
    e.preventDefault();
    if (!categoryName.value) {
      message.warn('分类名称不能为空');
      return;
    }

    categoryLoading.value = true;
    const newCategory = {
      id: categoryList.value.length + 1,
      categoryName: categoryName.value,
    };

    setTimeout(() => {
      categoryList.value.push(newCategory);
      categoryLoading.value = false;
      categoryName.value = '';
      message.success('分类添加成功');
    }, 500);
  }

  // 添加标签
  function addTagFunc(e) {
    e.preventDefault();
    if (!tagName.value) {
      message.warn('标签名称不能为空');
      return;
    }

    tagLoading.value = true;
    const newTag = {
      id: tagList.value.length + 1,
      tagName: tagName.value,
    };

    setTimeout(() => {
      tagList.value.push(newTag);
      tagLoading.value = false;
      tagName.value = '';
      message.success('标签添加成功');
    }, 500);
  }

  async function beforeUpload(file) {
    const isJpgOrPng = file.type === 'image/jpeg' || file.type === 'image/png' || file.type === 'image/webp'
    if (!isJpgOrPng) {
      message.error('文件格式必须是jpg或png或webp')
      return false
    }

    const isLt5M = file.size / 1024 / 1024 < 5
    if (!isLt5M) {
      message.error('图片必须小于 3MB')
      return false
    }

    fileList.value = [file]

    const base64 = await getBase64(file);
    previewBase64.value = base64;

    return false
  }

  // 提交表单
  function onFinish() {
    if (!formData.value.articleTitle || !formData.value.categoryId || !formData.value.tagId || !formData.value.articleContent) {
      message.warn('请检查是否填写完整');
      return;
    }

    if (!formData.value.articleCover && !fileList.value[0]) {
      message.warn('请上传文章封面');
      return;
    }

    console.log(formData);
    message.success('发布成功');
  }


  function close() {
    multiTab.close(route.fullPath)
  }

  const toolbars = [
    'bold',
    'underline',
    'italic',
    '-',
    'title',
    'strikeThrough',
    'sub',
    'sup',
    'quote',
    'unorderedList',
    'orderedList',
    'task',
    '-',
    'codeRow',
    'code',
    'link',
    'image',
    'table',
    'mermaid',
    'katex',
    '-',
    'revoke',
    'next',
    'save',
    '=',
    'pageFullscreen',
    'fullscreen',
    'preview',
    'htmlPreview',
    'catalog',
  ]

  //富文本图片上传处理 
  function onUploadArticleImg() {

  }
</script>

<template>
  <div class="publist">
    <div class="crumbs">
      <a-breadcrumb>
        <a-breadcrumb-item>网站管理</a-breadcrumb-item>
        <a-breadcrumb-item>文章管理</a-breadcrumb-item>
        <a-breadcrumb-item>发布文章</a-breadcrumb-item>
      </a-breadcrumb>
    </div>
    <layoutTop :is-list="false">
      <template #form-items>

        <a-form-item label="标题" style="margin-right: 1rem">
          <a-input v-model:value="formData.articleTitle" placeholder="输入文章标题" style="width: 15em" />
        </a-form-item>

        <a-form-item label="分类" style="margin-right: 1rem">
          <a-space>
            <a-select v-if="categoryList" v-model:value="formData.categoryId" :loading="categoryLoading" placeholder="选择分类" style="width: 15em" :options="categoryList.map(item => ({ value: item.id, label: item.categoryName }))">
              <template #dropdownRender="{ menuNode: menu }">
                <VNodes :vnodes="menu" />
                <a-divider style="margin: 4px 0" />
                <a-space style="padding: 4px 8px">
                  <a-input ref="inputRef" v-model:value="categoryName" placeholder="添加分类" />
                  <a-button type="text" @click="addCategoryFunc">
                    <template #icon>
                      <plus-outlined />
                    </template>
                    添加
                  </a-button>
                </a-space>
              </template>
            </a-select>
          </a-space>
        </a-form-item>

        <a-form-item label="标签" style="margin-right: 1rem">
          <a-select v-if="tagList" v-model:value="formData.tagId" mode="multiple" :loading="tagLoading" placeholder="选择标签" style="width: 15em" :options="tagList.map(item => ({ value: item.id, label: item.tagName }))">
            <template #dropdownRender="{ menuNode: menu }">
              <VNodes :vnodes="menu" />
              <a-divider style="margin: 4px 0" />
              <a-space style="padding: 4px 8px">
                <a-input ref="inputRef" v-model:value="tagName" placeholder="添加标签" />
                <a-button type="text" @click="addTagFunc">
                  <template #icon>
                    <plus-outlined />
                  </template>
                  添加
                </a-button>
              </a-space>
            </template>
          </a-select>
        </a-form-item>

        <a-form-item label="类型" style="margin-right: 1rem">
          <a-select v-model:value="formData.articleType" style="width: 15em">
            <a-select-option :value="1">
              原创
            </a-select-option>
            <a-select-option :value="2">
              转载
            </a-select-option>
            <a-select-option :value="3">
              翻译
            </a-select-option>
          </a-select>
        </a-form-item>

        <a-form-item label="状态" style="margin-right: 1rem">
          <a-space>
            <a-select v-model:value="formData.status" style="width: 120px">
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

        <a-form-item label="是否顶置" style="margin-right: 1rem">
          <a-select v-model:value="formData.isTop" style="width: 13em">
            <a-select-option :value="1">
              是
            </a-select-option>
            <a-select-option :value="0">
              否
            </a-select-option>
          </a-select>
        </a-form-item>

        <a-form-item>
          <a-space>
            <template v-if="previewBase64 || formData.articleCover">
              <a-popover title="预览">
                <template #content>
                  <a-image :width="200" :src="previewBase64 || formData.articleCover" />
                </template>
                <a-upload :file-list="fileList" :before-upload="beforeUpload" :max-count="1" :show-upload-list="false">
                  <a-button>
                    <PictureOutlined />
                    上传封面
                  </a-button>
                </a-upload>
              </a-popover>
            </template>
            <template v-else>
              <a-upload :file-list="fileList" :before-upload="beforeUpload" :max-count="1" :show-upload-list="{ showRemoveIcon: false }">
                <a-button>
                  <PictureOutlined />
                  上传封面
                </a-button>
              </a-upload>
            </template>
            <a-button type="primary" @click="onFinish">
              发布
            </a-button>
            <a-button class="orange" style="margin-right: 10px" @click="close">
              <template #icon>
                <CloseOutlined />
              </template>
              关闭
            </a-button>
          </a-space>
        </a-form-item>
      </template>
      <template #table-content>
        <div style="height: 80vh;width: 100%">
          <MdEditor v-model="formData.articleContent" :theme="mode" style="height: 80vh" :toolbars="toolbars" @onUploadImg="onUploadArticleImg" />
        </div>
      </template>
    </layoutTop>
  </div>
</template>

<style lang="scss" scoped>
.publist{
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

:deep(.anticon-plus){
  svg{
    margin-top: -0.35rem;
  }
}
</style>