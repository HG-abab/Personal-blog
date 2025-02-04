<template>
  <div class="photoa">
    <div style="margin-left:15%;margin-right:10%;height:100%">
      <div class="left">
        <el-menu default-active="1" class="el-menu-vertical-demo" @select="handleSelect" @open="handleOpen" @close="handleClose">
          <el-menu-item index="1">
            <div class="boder active-boder border">
              <SvgIcon name="house" style="margin-right: 0.3rem" />
              <span>主页</span>
            </div>
          </el-menu-item>
          <el-sub-menu index="2">
            <template #title>
              <span style="margin-left: 0.5rem">MC实况</span>
            </template>
            <el-sub-menu index="3" class="el-el-sub">
              <template #title>GTNH</template>
              <el-menu-item class="border" index="4">
                <SvgIcon name="folder" style="margin-right: 0.3rem" />
                产线图
              </el-menu-item>
            </el-sub-menu>
            <el-menu-item class="border" index="5">
              <SvgIcon name="folder" style="margin-right: 0.3rem" />
              GTL
            </el-menu-item>
            <el-menu-item class="border" index="6">
              <SvgIcon name="folder" style="margin-right: 0.3rem" />
              风景图
            </el-menu-item>
          </el-sub-menu>
          <el-sub-menu index="7">
            <template #title>
              <span style="margin-left: 0.5rem">美食美图</span>
            </template>
            <el-sub-menu index="8" class="el-el-sub">
              <template #title>风景</template>
              <el-menu-item class="border" index="9">
                <SvgIcon name="folder" style="margin-right: 0.3rem" />
                精品图
              </el-menu-item>
            </el-sub-menu>
            <el-sub-menu index="10" class="el-el-sub">
              <template #title>美食</template>
              <el-menu-item class="border" index="11">
                <SvgIcon name="folder" style="margin-right: 0.3rem" />
                精品图
              </el-menu-item>
            </el-sub-menu>
          </el-sub-menu>
          <el-menu-item index="12">
            <div class="boder border">
              <SvgIcon name="folder" style="margin-right: 0.3rem" />
              <span>精选集</span>
            </div>
          </el-menu-item>
          <el-menu-item index="13">
            <div class="boder border">
              <SvgIcon name="folder" style="margin-right: 0.3rem" />
              <span>测试集</span>
            </div>
          </el-menu-item>
        </el-menu>
      </div>
    </div>
    <div class="right">
      <div class="right-top" :style="{
        'background-image': 'url(' + Carousel7 + ')'
      }">
        <div style="backdrop-filter: blur(4px);;border-radius: 20px; width: 100%; height: 100%;"></div>
        <!-- 面包屑 -->
        <div class="breadcrumb">
          <el-breadcrumb separator="/">
            <el-breadcrumb-item>
              <div class="breadcrumb-item">
                <SvgIcon name="house" style="margin-right: 0.1rem" />
                主页
              </div>
            </el-breadcrumb-item>
          </el-breadcrumb>
        </div>
        <div class="banner-title">
          <span>我的相册</span>
        </div>
        <div class="stats-group">
          <div class="stats-item">
            <SvgIcon name="camera" style="margin-right: 0.3rem" />
            <span>12张照片</span>
          </div>
          <div class="stats-item">
            <SvgIcon name="folder" style="margin-right: 0.3rem" />
            <span>6个相册</span>
          </div>
        </div>
      </div>
      <div class="right-bottom">
        <!--相册 -->
        <div v-for="item in images" class="imgbox">
          <div v-if="item.id === 0">
            <img v-lazy="item.src" alt="" />
            <div class="imgbox-title">
              <SvgIcon name="folder" style="margin-right: 0.3rem" />
              <span>精品</span>
            </div>
          </div>
          <div v-viewer>
            <img v-if="item.id !== 0" v-lazy="item.src" :key="item.id" :alt="item.alt" />
          </div>
        </div>
      </div>
    </div>
  </div>
  <Foter />
</template>
<script setup>
  import Foter from '@/components/Foter/index.vue';
  import Carousel1 from '@/assets/imgs/Carousel01.png';
  import Carousel2 from '@/assets/imgs/Carousel02.png';
  import Carousel3 from '@/assets/imgs/Carousel03.png';
  import Carousel4 from '@/assets/imgs/1.png';
  import Carousel5 from '@/assets/imgs/2.png';
  import Carousel6 from '@/assets/imgs/3.png';
  import Carousel7 from '@/assets/imgs/4.png';
  import Carousel8 from '@/assets/imgs/5.png';

  import {
    Document,
    Menu as IconMenu,
    Location,
    Setting,
  } from '@element-plus/icons-vue'
  import { onMounted, ref } from 'vue';

  onMounted(() => {
    const elsubtitleEl = document.querySelectorAll('.el-sub-menu__title')
    elsubtitleEl.forEach((item) => {
      item.classList.add('border')
    })
  })

  // 将导入的图片数组转换为对象数组
  const images = ref([
    { id: 0, src: "https://pic2.zhimg.com/v2-3aecfb4a857585d6eb2796902a565956_r.jpg?source=172ae18b", alt: '文件' },
    { id: 1, src: Carousel1, alt: '图片1' },
    { id: 2, src: Carousel2, alt: '图片2' },
    { id: 3, src: Carousel3, alt: '图片3' },
    { id: 4, src: Carousel4, alt: '图片4' },
    { id: 5, src: Carousel5, alt: '图片5' },
    { id: 6, src: Carousel6, alt: '图片6' },
    { id: 7, src: Carousel7, alt: '图片7' },
    { id: 8, src: Carousel8, alt: '图片8' },

  ]);

  const handleOpen = (key) => {
    const borderEl = document.querySelectorAll('.border')
    console.log(borderEl)
    borderEl.forEach((item, index) => {
      item.style.background = 'var(--mao-item-bg)'
      item.style.color = 'var(--mao-album-color)'
      item.classList.remove('active-boder')
      if (index + 1 == key) {
        item.classList.add('active-boder')
        item.style.background = '#5c6ac4'
        item.style.color = '#ffffffe6'
      }
    })
  }
  const handleClose = (key) => {
    // console.log(key, keyPath)
  }

  const handleSelect = (key) => {
    const borderEl = document.querySelectorAll('.border')
    borderEl.forEach((item, index) => {
      item.style.background = 'var(--mao-item-bg)'
      item.style.color = 'var(--mao-album-color)'
      item.classList.remove('active-boder')
      if (index + 1 == key) {
        item.classList.add('active-boder')
        item.style.background = '#5c6ac4'
        item.style.color = '#ffffffe6'
      }
    })
  }

</script>

<style lang="scss" scoped>
.photoa{
  display: flex;
  padding-top:50px;
  height: 100vh;
  background-color: var(--mao-background-color);
  .left{
    width: 11vw;
    height: 100px;
    position: sticky;
    top: 0px;
  }
  .right{
    width: 60%;
    border-radius: 20px;
    margin-left: -7rem;
    margin-bottom: 1rem;
    .right-top{
      width: 100%;
      height: 30%;
      border-radius: 20px;
      background-size: cover;
      background-repeat: no-repeat;
      background-position: center;
      position: relative;
      .breadcrumb{
        position: absolute;
        top: 1rem;
        left: 1rem;
        border-radius: 20px;
        padding:0.6rem 1rem ;
        backdrop-filter: blur(10px);
        background: rgba(255, 255, 255, .1);
        .breadcrumb-item{
          display: flex;
          align-items: center;
          color: #ffffffe6;
          border-radius: 5px;
          padding:3px 3px;
          transition: all 0.5s ease-in-out;
          transform: scale(1);
          &:hover{
            transform: scale(1.05);
            background-color: #94989a;
          }
        }
      }
      .banner-title{
        font-size: 3.2em;
        font-weight: 700;
        margin: 0 0 16px;
        text-shadow: 0 2px 8px rgba(0, 0, 0, .3);
        letter-spacing: -.02em;
        line-height: 1.2;
        color: #ffffffe6;
        position: absolute;
        top: 35%;
        left: 5%;
      }
      .stats-group{
        display: flex;
        position: absolute;
        top: 70%;
        left: 5%;
        .stats-item{
          margin-right: 1rem;
          background: rgba(255, 255, 255, .2);
          padding: 0.8rem 1rem;
          border-radius: 20px;
          color: #ffffffe6;
          display: flex;
          transition: all 0.2s ease-in-out;
          transform: scale(1);
          &:hover{
            transform: scale(1.05);
            background-color: #94989a;
          }
        }
      }
    }
    .right-bottom{
      flex-wrap: wrap;
      padding: 1rem;
      height: 64.5%;
      border-radius: 20px;
      margin-top: 1.6%;
      background-color: var(--mao-camera-bg);
      overflow-y: auto;
      overflow-x: hidden;
      display: flex;
      flex-wrap: wrap;
      .imgbox{
        position: relative;
        border-radius: 20px;
        display: flex;
        width: calc(25% - 1rem);
        height: calc(50% - 1rem);
        margin: 0.5rem;
        overflow: hidden;
        img{
          width: 100%;
          height: 100%;
          border-radius: 20px;
          object-fit: cover;
          transition: all 0.4s ease-in-out;
          &:hover{
          transform: scale(1.05);
          box-shadow: 0 0 10px 0 #0000001a;
          }
        }
        .imgbox-title{
          position: absolute;
          bottom: 2vh;
          left: 1vw;
          text-align: center;
          color: #ffffffe6;
          display: flex;
        }
      }
    }
  }
}

.el-menu{
  height: 90vh;
  border-radius: 10px;
  background-color: var(--mao-bg-album);
  overflow: hidden;
}

.el-menu-item{
  border-radius: 10px;
  display: flex;
  height: 45px;
  color: var(--mao-album-color);
  font-size: 0.9rem;
  display: flex;
  justify-content: center;
  &:hover{
    background-color: transparent;
  }
}

.el-menu-item:nth-child(1){
  margin-bottom: 0.9rem;
}

.el-menu-item:nth-child(4){
  margin-top: -0.2rem;
}

.boder{
  width: 100%;
  height: 80%;
  display: flex;
  align-items: center;
  border: 1px solid rgba(224, 214, 206, 0.1);
  background-color: var(--mao-item-bg);
  border-radius: 10px;
  padding-left: 0.4rem;
  margin-top: 1rem;
}

.active-boder {
  width: 100%;
  height: 80%;
  display: flex;
  align-items: center;
  border: 1px solid rgba(224, 214, 206, 0.1);
  background-color: #5c6ac4;
  border-radius: 10px;
  padding-left: 0.4rem;
  margin-top: 1rem;
  color: #ffffffe6;
}

:deep(.el-sub-menu__title){
  background-color: var(--mao-item-bg);
  height: 36px;
  border-radius: 10px;
  margin: 0 1.3rem;
  margin-top: 0.5rem;
  padding-left: 0;
  color: var(--mao-album-color);
  .el-icon{
    position: absolute;
    top: 50%;
    left: -70%;
  }
}
.el-sub-menu .el-menu-item{
  background-color: var(--mao-item-bg);
  height: 36px;
  border-radius: 10px;
  margin: 0 1.3rem;
  margin-top: 0.5rem;
  color: var(--mao-album-color);
  display: flex;
  justify-content: flex-start;
  padding: 0 !important;
  padding-left: 1rem !important;
  overflow: hidden;
  position: relative;
}
:deep(.el-menu--inline){
  background-color: var(--mao-bg-album);
  margin-left: 1rem
}
:deep(.el-el-sub .el-sub-menu__icon-arrow){
  position: absolute;
  top: 50%;
  left: -55%;
  transform: translate(-50%, -50%);
}
:deep(.el-breadcrumb__inner){
  color: #ffffffe6;
}
</style>