<script setup>
  import { ref, onMounted } from 'vue';
  import { message } from 'ant-design-vue';
  import { VueDraggable } from 'vue-draggable-plus';
  import { ZoomInOutlined, DeleteOutlined, PlusOutlined } from '@ant-design/icons-vue';

  // 导入本地图片
  import imgbg1 from '@/assets/images/1.png';
  import imgbg2 from '@/assets/images/2.png';
  import imgbg3 from '@/assets/images/3.png';
  import imgbg4 from '@/assets/images/4.png';
  import imgbg5 from '@/assets/images/5.png';
  import imgbg6 from '@/assets/images/6.png';

  // 图片列表
  const fileList = ref([
    { id: '1', path: imgbg1, size: '1024', type: 'image/png', userId: 'user1', sortOrder: 1, createTime: '2023-10-01' },
    { id: '2', path: imgbg2, size: '2048', type: 'image/png', userId: 'user2', sortOrder: 2, createTime: '2023-10-02' },
    { id: '3', path: imgbg3, size: '3072', type: 'image/png', userId: 'user3', sortOrder: 3, createTime: '2023-10-03' },
    { id: '4', path: imgbg4, size: '4096', type: 'image/png', userId: 'user4', sortOrder: 4, createTime: '2023-10-04' },
    { id: '5', path: imgbg5, size: '5120', type: 'image/png', userId: 'user5', sortOrder: 5, createTime: '2023-10-05' },
    { id: '6', path: imgbg6, size: '6144', type: 'image/png', userId: 'user6', sortOrder: 6, createTime: '2023-10-06' },
  ]);

  const el = ref(null);
  const disabled = ref(false);

  // 预览图片
  const previewVisible = ref(false);
  // 预览图片的当前数据
  const tempImage = ref(null);
  // 预览图片的标题
  const previewTitle = ref('');

  // 获取文件的 Base64 编码
  function getBase64(file) {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.readAsDataURL(file);
      reader.onload = () => resolve(reader.result);
      reader.onerror = error => reject(error);
    });
  }

  // 关闭预览
  function handleCancel() {
    previewVisible.value = false;
    previewTitle.value = '';
  }

  // 预览图片
  async function handlePreview(file) {
    if (!file.path && !file.preview) {
      file.preview = await getBase64(file.originFileObj);
    }
    tempImage.value = file;
    previewVisible.value = true;
    previewTitle.value = file.name || file.path.substring(file.path.lastIndexOf('/') + 1);
  }

  // 上传图片
  const uploading = ref(false);
  const progress = ref(0);

  // 上传前校验
  async function beforeUpload(file) {
    const isJpgOrPng = file.type === 'image/jpeg' || file.type === 'image/png' || file.type === 'image/webp';
    if (!isJpgOrPng) {
      message.error('文件格式必须是jpg或png或webp');
      return;
    }

    const isLt400KB = file.size / 1024 < 400;
    if (!isLt400KB) {
      message.error('图片必须小于 0.3MB');
      return;
    }

    // 模拟上传
    uploading.value = true;
    setTimeout(() => {
      const newFile = {
        id: String(fileList.value.length + 1),
        path: URL.createObjectURL(file),
        size: file.size,
        type: file.type,
        userId: 'user' + (fileList.value.length + 1),
        sortOrder: fileList.value.length + 1,
        createTime: new Date().toISOString(),
      };
      fileList.value.push(newFile);
      uploading.value = false;
      message.success('上传成功');
      console.log('上传成功', newFile);
    }, 1000);

    return false;
  }

  // 删除图片
  function deleteBannerFunc(bannerId) {
    fileList.value = fileList.value.filter(file => file.id !== bannerId);
    message.success('删除成功');
  }

  // 排序
  function sortOrderFunc() {
    message.success('排序成功');
  }
</script>

<template>
  <a-divider>首页轮播图</a-divider>
  <div class="carousel">
    <div class="flex">
      <VueDraggable :disabled="disabled" ref="el" v-model="fileList" :onEnd="sortOrderFunc" style="display: flex" animation="150">
        <template v-for="file in fileList" :key="file.id">
          <div class="preview_list">
            <div class="m-2 w-[100px] h-[100px]">
              <img :src="file.path" alt="banners" class="w-full h-full  rounded-2xl ">
            </div>
            <!-- 灰色遮盖层 -->
            <div class="grey_cover m-2 w-[100px] h-[100px] bg-gray-300 rounded-2xl relative">
              <div class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 text-black flex">
                <div style="font-size: 20px;" class="text-gray-400 hover:text-black duration-300" @click="handlePreview(file)">
                  <ZoomInOutlined />
                </div>
                <div class="mx-1"></div>
                <div style="font-size: 20px;" class="text-gray-400 hover:text-black duration-300" @click="deleteBannerFunc(file.id)">
                  <DeleteOutlined />
                </div>
              </div>
            </div>
          </div>
        </template>
        <div class="flex flex-col justify-center items-center">
          <div v-if="uploading" class="m-2 w-[100px] h-[100px] text-gray-400 flex justify-center items-center rounded-2xl bg-gray-300">
            上传中...
          </div>
          <a-progress :show-info="false" v-if="uploading" :percent="progress ?? 0" size="small" class="ml-1" />
        </div>
      </VueDraggable>
      <!-- 上传按钮 -->
      <a-upload class="ant-upload ant-upload-picture-card m-2" v-model="fileList" list-type="picture-card" :showUploadList="false" :before-upload="beforeUpload" v-show="fileList && fileList.length < 5">
        <div class="el-upload el-upload--picture-card">
          <div class="icon">
            <PlusOutlined />
          </div>
        </div>
      </a-upload>
    </div>
    <a-modal :open="previewVisible" :footer="null" @cancel="handleCancel">
      <template #title>
        <span style="font-size: 1rem">名称：{{ previewTitle }}</span> <br>
        <div>大小： {{ (tempImage.size / 1024).toFixed(2) }} KB</div>
        <div>上传时间： {{ tempImage.createTime }}</div>
        <div>格式： {{ tempImage.type }}</div>
        <div>上传人id： {{ tempImage.userId }}</div>
      </template>
      <img alt="example" style="width: 100%" :src="tempImage.path">
    </a-modal>
  </div>
</template>

<style scoped lang="scss">
.preview_list {
  position: relative;

  .grey_cover {
    position: absolute;
    top: 0;
    left: 0;
    width: 100px;
    height: 100px;
    opacity: 0;
  }

  &:hover .grey_cover {
    opacity: 0.8;
    transition: all 0.3s ease;
  }
}

.carousel {
  display: flex;
  justify-content: center;
}

/* 基本样式 */
.el-upload {
  display: inline-block;
  text-align: center;
  cursor: pointer;
  outline: 0;

  .icon {
    font-size: 30px;
    color: #c0ccda;
  }
}

.el-upload--picture-card {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #FAFAFA;
  border: 5px dashed #D9D9D9;
  border-radius: 6px;
  box-sizing: border-box;
  width: 100px;
  height: 100px;
  line-height: 146px;
  vertical-align: top;
  transition: all 0.3s ease;

  &:hover {
    border-color: #409EFF;
  }
}

.el-upload__input {
  display: none;
}

/* 图标样式 */
.el-icon-plus {
  width: 30px;
  height: 30px;
  font-size: 30px;
  color: #c0ccda;
}

.ant-divider{
  padding: 0 26%;
}
</style>