<template>
  <div class="search_dialog_container">
    <!-- 搜索内容 -->
    <el-dialog v-model="dialogVisible" :show-close="false" :close-on-click-modal="false" :lock-scroll="true">
      <template #header>
        <div style="display: flex;justify-content: space-between;align-items: center;">
          <span style="font-size: 1.3rem; font-weight: 600; margin-left: 6px">搜索</span>
          <el-button :icon="Close" style="background: none;font-size: 1.4rem;width: 30px;border: none" @click="dialogVisible = false" />
        </div>
      </template>
      <navSearch @isShowSearch="dialogVisible = false" />
    </el-dialog>
  </div>
  <transition name="fade">
    <div class="navigation" v-if="isHidden">
      <div class="left">
        <el-link href="/" style="cursor:default;margin-left:4px; padding-right: 8px" :underline="false">Moyu-开摆</el-link>
        <div class="Home">
          <el-link href="/" :underline="false" class="nav_link">
            <el-icon class="elicon" :size="18" style="margin-top: -1px">
              <HomeFilled />
            </el-icon>
            <span class="text">首页</span>
          </el-link>
        </div>
        <div class="nav_item">
          <el-link :underline="false">
            <pigeOnhole />
          </el-link>
        </div>
        <div class="nav_item">
          <el-link :underline="false">
            <other />
          </el-link>
        </div>
        <div class="Home">
          <el-link href="/Friendchains" :underline="false">
            <el-icon class="elicon" :size="18" style="margin-top: -1px">
              <Paperclip />
            </el-icon>
            <span class="text">友链</span>
          </el-link>
        </div>
        <div class="Home">
          <el-link href="/photoa" :underline="false">
            <el-icon class="elicon" :size="18" style="margin-top: -1px">
              <UserFilled />
            </el-icon>
            <span class="text">相册</span>
          </el-link>
        </div>
        <div class="Home">
          <el-link href="/concerning" :underline="false">
            <el-icon class="elicon" :size="18" style="margin-top: -1px">
              <Tools />
            </el-icon>
            <span class="text">关于</span>
          </el-link>
        </div>
      </div>
      <div class="right">
        <div class="toggle">
          <toggle-button @change="changeToggle" size="1"></toggle-button>
        </div>
        <div class="search" @click="dialogVisible = true">
          <SvgIcon name="search" width="2em" height="2em" color="#409EFF" class="icon" />
        </div>
      </div>
      <!-- 移动端 -->
      <div class="move_nav" style="margin-left: 0.7rem">
        <div class="left">
          <div @click="drawer = true" style="margin-left: 0.5rem;">
            <SvgIcon name="directory_icon" width="26" height="26" color="#409EFF" class="icon" />
          </div>
          <!-- 移动端日夜切换 -->
          <div style="margin-left: 3.5rem;">
            <toggle-button @change="changeToggle"></toggle-button>
          </div>
        </div>
        <div class="right">
          <div class="search" @click="dialogVisible = true">
            <SvgIcon name="search" width="2em" height="2em" color="#409EFF" class="icon" />
          </div>
        </div>
      </div>
    </div>
  </transition>
  <div>
    <el-drawer v-model="drawer" :with-header="true" size="40%" direction="ltr" :show-close="false">
      <template #header>
        <span style="font-size: 1.2rem">导航</span>
        <el-button :icon="Close" style="background: none;font-size: 1.5rem;width: 30px;border: none" @click="drawer = false" />
      </template>
      <template #default>
        <Mobile @Jump:closeDrawer="drawer = false" />
      </template>
    </el-drawer>
  </div>
</template>


<script setup>
  import { onMounted, onUnmounted, ref } from 'vue'
  import { User, Close, Paperclip, UserFilled, Headset, Tools } from '@element-plus/icons-vue'
  import { HomeFilled } from '@element-plus/icons-vue'
  import pigeOnhole from './nav-item/nav-item-pigeonhole.vue'
  import other from './nav-item/nav-item-other.vue'
  import DayNightToggleButton from '../DayNightToggle/index'
  import { useColorMode } from '@vueuse/core'
  import navSearch from '../navSearch/index.vue'
  import Mobile from './Mobilenavigation/index.vue'

  onMounted(async () => {
    customElements.define("toggle-button", DayNightToggleButton);
  })


  const dialogVisible = ref(false)

  const mode = useColorMode()
  function changeToggle({ detail }) {
    mode.value = detail
  }
  const isHidden = ref(true)
  const drawer = ref(false)


  const handleWheelEvent = (event) => {
    if (event.deltaY < 0) {
      isHidden.value = true;
      console.log(isHidden.value)
    } else if (event.deltaY > 0) {
      isHidden.value = false;
      console.log(isHidden.value)
    }
  };

  onMounted(() => {
    window.addEventListener('wheel', handleWheelEvent);
  });

  // 在组件卸载时移除事件监听器
  onUnmounted(() => {
    window.removeEventListener('wheel', handleWheelEvent);
  });
</script>
<style lang="scss" scoped>
:deep(.el-dialog) {
  width: 50vw;
  height: 70vh;
  padding-top: 0;
  padding:0 8px;
  // 过渡效果
  transition: all .3s;
  @media (max-width: 1400px) {
    width: 45%;
  }
  @media (max-width: 1000px) {
    width: 60%;
  }
  @media (max-width: 760px) {
    width: 70%;
  }
  @media (max-width: 600px) {
    width: 90%;
  }
  @media screen and (max-width: 650px) {
      border-radius: 0;
      margin-top: 0;
      margin-bottom: 0;
      width: 100vw;
      height: 100%;
  }
}
:deep(.el-dialog__header){
  padding: 0.8rem 0;
}
.navigation {
  width: 100vw;
  height: 50px;
  // 朦胧
  backdrop-filter: blur(10px);
  background-color: rgba(255, 255, 255, 0.041);
  position: fixed;
  top: 0;
  left: 0;
  z-index: 999;
  display: flex;
  justify-content:space-between;
  align-items: center;
  transition: transform 0.3s ease-in-out;
  .left {
    display: flex;
    justify-content: space-between;
    align-items: center;
    @media screen and (max-width: 910px) {
      display: none;
    }
    .el-link{
      color: var(--mao-hearder-color);
      font-size: 1.2rem;
      font-weight: 700;
      margin: 0 6px;
    }
    .Home{
      position: relative;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 8px;
      &::before {
      content: ''; // 必须设置content属性
      position: absolute;
      bottom: -69%; // 进度条位于悬浮框上方
      left: 0;
      width: 100%; // 进度条宽度为100%
      height: 1.6px; // 进度条高度
      background-color: rgb(64, 158, 255); // 进度条颜色
      transform: scaleX(0); // 初始状态缩放为0，即不可见
      transform-origin: left; // 缩放起点在左侧
      transition: transform 0.5s ease-in-out; // 缩放过渡效果
    }
    &:hover::before {
    transform: scaleX(1); // 缩放变为1，即显示完整的进度条
  }
      .text{
        font-size: 1rem;
        margin-left: 2px;
      }
    }
    .nav_item{
      display: flex;
    }
  }
  .right{
    display: flex;
    align-items: center;
    @media screen and (max-width: 910px) {
      display: none;
    }
    .toggle{
      margin-right: 6vw;
    }
    .search{
      margin-right: 4vw;
      margin-top: 3px;
      transform: scale(1);
      transition: all 0.3s ease-in-out;
      // 鼠标放上去 放大1.2
      &:hover{
        transform: scale(1.2);
        // 动画
        transition: all 0.3s ease-in-out;
      }
    }
  }
}
.elicon{
  color: var(--mao-hearder-color);
}

.fade-enter-active, .fade-leave-active {
  transition: all 0.3s ease-in-out;
  backdrop-filter: blur(10px);
}
.fade-enter-from, .fade-leave-to {
  transition: all 0.3s ease-in-out;
  transform: translateY(-100%);
  backdrop-filter: blur(10px);
  background-color: rgba(255, 255, 255, 0.473);
}

.move_nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 50px;
  position: fixed;
  top: 0;
  z-index: 999;
  width: 100vw;
  @media screen and (min-width: 910px) {
    display: none;
  }
  .left{
    display: flex;
    align-items: center;
  }
  .right {
    display: flex;
  }
}

</style>
