<template>
  <div>
    <a-layout id="components-layout-demo-top-side-2">
      <a-layout-header class="header">
        <div class="logo">
          <SvgIcon name="hdm" width="26px" height="26px"></SvgIcon>
          <span>Moyu-blog</span>
        </div>
        <div>
          <a-menu v-model="current" mode="horizontal">
            <a-menu-item key="mail">
              <div style="height: 100%;display: flex;align-items: center;justify-content: center">
                <SvgIcon name="appstore" class="icon"></SvgIcon>
                <span>网站管理</span>
              </div>
            </a-menu-item>
          </a-menu>
        </div>
      </a-layout-header>
      <a-layout>
        <a-layout-sider class="sider" v-model:collapsed="collapsed" collapsible>
          <a-menu v-model:selectedKeys="selectedKeys" mode="inline">
            <router-link to="/">
              <a-menu-item key="1">
                <template #icon>
                  <FileSearchOutlined />
                </template>
                信息管理
              </a-menu-item>
            </router-link>
            <a-sub-menu key="sub1">
              <template #icon>
                <BookOutlined />
              </template>
              <template #title>文章管理</template>
              <router-link to="/list">
                <a-menu-item key="2">
                  <template #icon>
                    <BarsOutlined />
                  </template>
                  文章列表
                </a-menu-item>
              </router-link>
              <router-link to="/publish">
                <a-menu-item key="3">
                  <template #icon>
                    <SendOutlined />
                  </template>
                  发布文章
                </a-menu-item>
              </router-link>
            </a-sub-menu>
            <router-link to="/message">
              <a-menu-item key="4">
                <template #icon>
                  <MessageOutlined />
                </template>
                留言管理
              </a-menu-item>
            </router-link>
            <router-link to="/tag">
              <a-menu-item key="5">
                <template #icon>
                  <TagsOutlined />
                </template>
                标签管理
              </a-menu-item>
            </router-link>
            <router-link to="/category">
              <a-menu-item key="6">
                <template #icon>
                  <FolderOpenOutlined />
                </template>
                分类管理
              </a-menu-item>
            </router-link>
            <router-link to="/collect">
              <a-menu-item key="7">
                <template #icon>
                  <FireOutlined />
                </template>
                收藏管理
              </a-menu-item>
            </router-link>
            <router-link to="/comment">
              <a-menu-item key="8">
                <template #icon>
                  <CommentOutlined />
                </template>
                评论管理
              </a-menu-item>
            </router-link>
            <router-link to="/tree-hole">
              <a-menu-item key="9">
                <template #icon>
                  <SlackOutlined />
                </template>
                树洞管理
              </a-menu-item>
            </router-link>
            <router-link to="/link">
              <a-menu-item key="10">
                <template #icon>
                  <NodeIndexOutlined />
                </template>
                友链管理
              </a-menu-item>
            </router-link>
          </a-menu>
        </a-layout-sider>
        <a-layout style="min-height: 100vh">
          <a-layout-content class="content">
            <router-tab></router-tab>
            <router-view>

            </router-view>
          </a-layout-content>
        </a-layout>
      </a-layout>
    </a-layout>
  </div>
</template>

<script setup>
  import { onMounted, ref, watch } from "vue";
  import { Menu } from 'ant-design-vue';
  import { useRouter, useRoute } from "vue-router";
  import {
    FileSearchOutlined,
    SendOutlined,
    BarsOutlined,
    BookOutlined,
    CommentOutlined,
    NodeIndexOutlined,
    SlackOutlined,
    FolderOpenOutlined,
    FireOutlined,
    MessageOutlined,
    TagsOutlined
  } from '@ant-design/icons-vue';


  const collapsed = ref(false);
  const selectedKeys = ref(['1']);
  const current = ref('mail');

  const urlmap = [
    { name: '', size: ['1'] },
    { name: 'list', size: ['2'] },
    { name: 'publish', size: ['3'] },
    { name: 'message', size: ['4'] },
    { name: 'tag', size: ['5'] },
    { name: 'category', size: ['6'] },
    { name: 'collect', size: ['7'] },
    { name: 'comment', size: ['8'] },
    { name: 'tree-hole', size: ['9'] },
    { name: 'link', size: ['10'] },
  ]

  onMounted(() => {
    const router = useRouter();
    urlmap.forEach((item) => {
      if (item.name === router.currentRoute.value.name) {
        selectedKeys.value = item.size
      }
    })
  })

  const router = useRouter();
  watch(() => router.currentRoute.value.name, (newVal) => {
    urlmap.forEach((item) => {
      if (item.name === newVal) {
        selectedKeys.value = item.size
      }
    })
  })
</script>

<style lang="scss" scoped>
.content{
  height: 100vh;
  background-color: rgb(240, 242, 245);
  overflow-y: auto;
}
.sider{
  background-color: #ffffff;
  .ant-menu{
    height: 90%;
  }
  .btton{
    display: flex;
    align-items: flex-end;
  }
  :deep(.ant-menu-item-selected){
    color: rgb(22 119 255) !important;
    background-color: #e6f4ff !important;
  }
  :deep(.ant-menu-item){
    color: #000;
    background-color: #ffffff;
    &:hover{
      background-color: #f0f0f0;
    }
  }
  :deep(.ant-menu .ant-menu-sub .ant-menu-item ){
       background-color: #fafafa;
  }
  :deep(.ant-layout-sider-trigger){
    color: #000;
    background-color: #ffffff;
  }
}
.header{
  display: flex;
  align-items: center;
  height: 5vh;
  padding-left: 1.5rem;
  overflow: hidden;
  .logo{
    display: flex;
    align-items: center;
    height: 100%;
    font-size: 1.2rem;
    font-weight: 600;
    color: #ffffff;
    span{
      margin-left: 0.5rem;
    }
  }
}

:deep(.ant-menu-overflow-item){
  margin: 0 !important;
  background-color: #001529;
  color: #ffffff;
  :hover{
  }
  .icon{
    margin-right: 0.5rem;
    margin-bottom: -0.15rem;
  }
}

:deep(.header .ant-menu){
  margin-left: 1rem;  
}

:deep(.ant-menu-item-selected){
  color: #ffffff !important;
  background-color: #1677ff !important;
}


</style>