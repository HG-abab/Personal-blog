<script setup>
  import { reactive, ref } from "vue";
  import avatarurl from '@/assets/images/Avatar.jpg'
  import avatarbg from '@/assets/images/image.png'

  const data = reactive(['站长信息', '网站信息'])
  const value = ref(data[0])
  import StationmasterInfo from './stationmaster-info/index.vue'
  import WebInfo from './web-info/index.vue'

  const isDark = ref(false)
  const info = ref({
    webmasterAvatar: avatarurl,
    webmasterName: '张三',
    webmasterCopy: '这是一个示例文案',
    webmasterProfileBackground: avatarbg,
    giteeLink: 'https://gitee.com/example',
    githubLink: 'https://github.com/example',
    websiteName: '我的博客',
    headerNotification: '欢迎访问我的博客！',
    sidebarAnnouncement: '这是一个示例公告，用于测试侧边栏公告功能。',
    recordInfo: '备案号：12345678',
    startTime: '2025-01-01T18:01:27+08:00',
    lastUpdateTime: '2025-1-19 12:00:00',
    articleCount: 100, // 文章数量
    wordCount: 50000, // 文章总字数
    visitCount: 10000, // 访问次数
    runTime: '280 天 12 小时 30 分钟 45 秒',
  })

  function resetStationmasterInfo() {
    info.value.webmasterName = undefined
    info.value.webmasterCopy = undefined
    info.value.githubLink = undefined
    info.value.giteeLink = undefined
  }

  function resetWebInfo() {
    // info.value.websiteName = undefined
    // info.value.headerNotification = undefined
    // info.value.sidebarAnnouncement = undefined
    // info.value.startTime = undefined
    // info.value.recordInfo = undefined
  }
</script>

<template>
  <div class="info">
    <div class="crumbs">
      <a-breadcrumb>
        <a-breadcrumb-item>网站管理</a-breadcrumb-item>
        <a-breadcrumb-item>信息管理</a-breadcrumb-item>
      </a-breadcrumb>
      <h1>信息管理</h1>
    </div>
    <div class="content">
      <div class="info_title">
        <a-segmented v-model:value="value" :options="data" />
      </div>
      <div>
        <div v-show="value === '站长信息'" class="info_container">
          <template v-if="info">
            <StationmasterInfo :info="info" @reset:info="resetStationmasterInfo" />
          </template>
        </div>
        <div v-show="value === '网站信息'" class="info_container">
          <template v-if="info">
            <WebInfo :info="info" @reset:web:info="resetWebInfo" />
          </template>
        </div>
      </div>
    </div>
  </div>

</template>

<style lang="scss" scoped>
.crumbs {
  padding: 1rem;
  background: white;
  h1{
    margin-top: 1rem;
    font-size: 1.2rem;
    font-weight: 500;
  }
}
.content{
  height: auto;
  margin: 1.5rem;
  padding-bottom: 2rem;
  margin-bottom: 5rem;
  background: white;
  .info_title{
    padding: 1rem;
  }
  @media screen and (max-width: 1400px) {
    .content{
      height: auto;
      margin-bottom: 4rem;
      background: white;
    }
  }
}

</style>