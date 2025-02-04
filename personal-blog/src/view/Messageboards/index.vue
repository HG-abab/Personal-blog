<template>
  <div is-side-bar style=" background-color: var(--bg--article);">
    <el-container class="common-layout" style="padding-top: 80px;">
      <el-container class="main">
        <router-view />
      </el-container>
      <el-aside class="aside" style="overflow: visible">
        <layoutAside />
        <Sidecard title="公告" prefixIcon="announcement" suffix-icon="jt_y" :isDithering="true" :isArrow="true" @invoke="announcement">
          <p class="cardpre">{{Filtercontent(announcementText)}}</p>
        </Sidecard>
        <Sidecard title="电子时钟" prefixIcon="time" :isScale="true" style="height: 110px;">
          <span style="font-family:Share Tech Mono; font-size: 0.9rem; margin:0 10px">{{getCurrentDate}}</span>
          <span style="font-family:Share Tech Mono;  font-size: 0.9rem">星期{{getWeekday}}</span>
          <p style="font-family:Share Tech Mono;font-size: 2.4rem; margin-top:20px">{{getTime}}</p>
        </Sidecard>
        <Sidecard title="网站资讯" prefixIcon="statistics" :isScale="true" style="height: 200px;">
          <div style="margin: 4px 5px; line-height: 1.7rem;">
            <div style="display: flex; justify-content: space-between; font-size:1.2rem;">
              <p>文章数目：</p>
              <p>8</p>
            </div>
            <div style="display: flex; justify-content: space-between;font-size:1.2rem">
              <p>运行时长：</p>
              <p>-60天</p>
            </div>
            <div style="display: flex; justify-content: space-between;font-size:1.2rem">
              <p>全站字数：</p>
              <p>1008611</p>
            </div>
            <div style="display: flex; justify-content: space-between;font-size:1.2rem">
              <p>访问次数：</p>
              <p>99</p>
            </div>
            <div style="display: flex; justify-content: space-between;font-size:1.2rem">
              <p>最后更新：</p>
              <p>1天前</p>
            </div>
          </div>
        </Sidecard>
        <Sidecard title="文章标签" prefixIcon="OIP-C" suffixIcon="rotate" :isScale="true" :isRotate="true" style="height:200px" @invoke="tagcloudcontainer">
          <div id="tag-cloud-container" class="custom-tagcloud"></div>
        </Sidecard>
      </el-aside>
    </el-container>
    <div style="margin-top: 4rem;"></div>
    <Foter />
  </div>
</template>

<script setup>
  import { ref, onMounted, onUnmounted } from 'vue';
  import { useRoute } from 'vue-router';
  import router from '@/router';
  import Foter from '@/components/Foter/index.vue'
  import useCurrentDateTime from '../../utils/time';
  import Sidecard from '@/components/Sidecards/index.vue'
  import { ElMessage, ElMessageBox } from 'element-plus'
  import { MdPreview, MdCatalog } from 'md-editor-v3';
  import 'md-editor-v3/lib/preview.css';
  import { useColorMode, useTitle } from "@vueuse/core";
  import TagCloud from '../../utils/TagCloud'

  // 富文本
  const id = 'preview-only';
  const text = ref(``);
  const scrollElement = document.documentElement;
  const mode = useColorMode()

  const announcementText = ref(`欢迎来到我的博客，
  在这里你可以看到我的一些文章和心得体会。如果你
  有任何问题或者建议，欢迎留言交流。欢迎来到我的博客，在这
  里你可以看到我的一些文章和心得体会。如果你有任何问题或者建议，欢
  迎留言交流。欢迎来到我的博客，在这里你可以看到我的一些文章和
  心得体会。如果你有任何问题或者建议，欢迎留言交流。欢迎来到我的博
  客，在这里你可以看到我的一些文章和心得体会。如果你有任何问题或者建
  议，欢迎留言交流。`);

  function announcement() {
    ElMessageBox.alert(`<p >${announcementText.value}</p>`, '公告', {
      confirmButtonText: '关闭',
      closeOnPressEscape: true,
      dangerouslyUseHTMLString: true,
      customClass: 'announcementText',
    })
  }

  const Filtercontent = (e) => {
    return e.substring(0, 80) + '...';
  }

  const { date: getCurrentDate, weekday: getWeekday, time: getTime } = useCurrentDateTime();

  const texts = ['前端', '编程', '优化', '工具', '后端'];
  const options = {
    radius: 70, // 设置滚动半径为70px
    maxSpeed: 'fast', // 设置最大滚动速度为快
    initSpeed: 'slow', // 设置初始滚动速度为慢
    direction: 45, // 设置初始滚动方向为45度
    reverseDirection: true, // 反转滚动方向
  };
  onMounted(() => {
    const tagCloudInstance = TagCloud('#tag-cloud-container', texts, options);
  })

  const tagcloudcontainer = () => {
    // 获取容器元素
    const container = document.getElementById('tag-cloud-container');
    // 如果容器存在，先清空其内容
    if (container) {
      container.innerHTML = '';
    }
    // 然后创建新的标签云
    TagCloud('#tag-cloud-container', texts, options);
  }
</script>

<style lang="scss" scoped>
.common-layout{
  margin: 0 12.8vw;
  @media screen and (max-width: 1600px) {
      margin: 0 7% ;
    }
  .main{
    border-radius: 8px;
    background-color: var(--mao-bg-content);
    .header{
      height: 340px;
      .header-img{
        border-radius: 15px;
        width: 100%;
        height: 100%;
        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;
        .text{
          padding: 60px 50px;
          color: #fff;
          .title{
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 45px;
            height: 35px;
            background-color: rgb(123, 115, 117,0.8);
          }
          .h1{
            font-size: 2.5rem;
            font-weight: 700;
            margin: 2.4vh 0;
          }
          .statistics{
            font-size: 14px;
            border-radius: 3px;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 110px;
            height: 36px;
            background-color: rgb(123, 115, 117,0.8);
          }
        }
      }
    }
    .footer{
      height: auto;
      margin: 20px 1vw;
      .copyright{
        border: 1px solid #ccc;
        border-radius: 10px;
        margin: 1rem 0;
        padding: 1rem 2rem;
        .license{
          display: flex;
          align-items: center;
          margin: 1rem 0;
          .license_text{
            font-size: 0.8rem;
          }
          .copyright_a{
            color: var(--a--color);
          }
        }
        .link{
          display: flex;
          align-items: center;
          margin: 1rem 0;
          .copyright_a{
            color: var(--a--color);
          }
        }
        .author{
          display: flex;
          align-items: center;
          margin: 1rem 0;
        }
      }
      .tipping-icon{
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: #fc7444;
        margin: 5vh 20vw;
        padding: 1.5vh 0;
        border-radius:25px ;
      }
      .goOn{
        display: flex;
        justify-content: space-between;
        margin: 1vh 0;
      }
    }
  }
  .aside{
      margin-left: 1vw;
      margin-right: 2vw;
      width: 22.5%;
      @media screen and (max-width: 1600px) {
        margin-right: 4vw;
      }
      @media screen and (max-width: 910px){
        display: none;
      }
    }
    .cardpre {
      text-align: left;
      font-size: 1rem;
      word-wrap: break-word;
      line-height: 1.2rem;
    }
}

//标签云
.custom-tagcloud{
  display: flex;
  justify-content: center;
}


</style>