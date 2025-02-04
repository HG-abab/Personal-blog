<template>
  <div class="common-layout">
    <el-container class="layout-container">
      <el-container class="layout-main">
        <el-header>
          <div class="announcement">
            <SvgIcon name="notice" color="#409EFF" height="18px" width="18px" />
            <span style="text">禁止发无关评论，违者永久封禁! ! !</span>
          </div>
        </el-header>
        <el-main>
          <div>
            <el-divider border-style="dashed" content-position="left">
              <div class="items-center">
                <SvgIcon name="recommend" color="#409EFF" height="14" width="14" style="margin-right: 5px" />
                <span>推荐</span>
              </div>
            </el-divider>
            <el-carousel motion-blur style="height: 200px">
              <el-carousel-item v-for="(item, index) in imageList" :key="index">
                <img v-lazy="item" :alt="'Image ' + index" class="carousel-image" />
                <div class="carousel-text">
                  <div class="title">项目更新日志</div>
                  <div class="timi">2024-12-8 15:22:21</div>
                  <div class="content">日期:2024/12/8v1.0具体提交曰志..</div>
                </div>
              </el-carousel-item>
            </el-carousel>
          </div>
          <div class="essay_title">
            <el-divider border-style="dashed" content-position="left">
              <div style="display: flex; align-items: center">
                <SvgIcon name="essay_icon" color="#409EFF" height="14" width="14" style="margin-right: 5px" />
                <span style="font-size: 0.9rem; margin-left: 0.2rem;">文章</span>
              </div>
            </el-divider>
          </div>
          <div>
            <Card />
          </div>
        </el-main>
        <el-footer>
          <el-divider border-style="dashed" content-position="center">
            <div class="footer-text" style="font-weight: bold">~~到达底部啦~~</div>
          </el-divider>
        </el-footer>
      </el-container>
      <el-aside class="layout-aside">
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
  import Carousel1 from '@/assets/imgs/Carousel01.png';
  import Carousel2 from '@/assets/imgs/Carousel02.png';
  import Carousel3 from '@/assets/imgs/Carousel03.png';
  import Card from '@/components/card/index.vue'
  import layoutAside from '@/components/layoutAside/index.vue'
  import Sidecard from '@/components/sideCards/index.vue'
  import { ElMessageBox } from 'element-plus'
  import useCurrentDateTime from '../../utils/time'
  import TagCloud from '../../utils/TagCloud'

  import { onMounted, ref } from 'vue';

  const { date: getCurrentDate, weekday: getWeekday, time: getTime } = useCurrentDateTime()

  const announcementText = ref(`欢迎来到我的博客，
  在这里你可以看到我的一些文章和心得体会。如果你
  有任何问题或者建议，欢迎留言交流。欢迎来到我的博客，在这
  里你可以看到我的一些文章和心得体会。如果你有任何问题或者建议，欢
  迎留言交流。欢迎来到我的博客，在这里你可以看到我的一些文章和
  心得体会。如果你有任何问题或者建议，欢迎留言交流。欢迎来到我的博
  客，在这里你可以看到我的一些文章和心得体会。如果你有任何问题或者建
  议，欢迎留言交流。`);

  const imageList = ref([
    Carousel1,
    Carousel2,
    Carousel3
  ]);
  const texts = ['前端', '编程', '优化', '工具', '后端'];
  const options = {
    radius: 70, // 设置滚动半径为150px
    maxSpeed: 'fast', // 设置最大滚动速度为快
    initSpeed: 'slow', // 设置初始滚动速度为慢
    direction: 45, // 设置初始滚动方向为45度
    reverseDirection: true, // 反转滚动方向
  };
  onMounted(() => {
    const tagCloudInstance = TagCloud('#tag-cloud-container', texts, options);
  })

  function announcement() {
    ElMessageBox.alert(`<p >${announcementText.value}</p>`, '公告', {
      confirmButtonText: '关闭',
      closeOnPressEscape: true,
      dangerouslyUseHTMLString: true,
      customClass: 'announcementText',
    })
  }

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

  const Filtercontent = (e) => {
    return e.substring(0, 80) + '...';
  }
</script>

<style lang="scss" scoped> 
.common-layout {
  position: absolute;
  top: 100%;
  width: 100%;
  transition: all 0.5s;
  background-color: var(--mao-background-color);
  .layout-container{
    margin: 0 12.8%;
    .el-header{
      margin: 1vh 0;
      padding: 0 1vw;
    }
    .el-footer{
      .footer-text{
        font-size: 1rem;
      }
    }
    .layout-main{
      border-radius: 5px;
      background-color: var(--mao-bg-content);
      .announcement{
        height: 4.5vh;
        margin-top: 10px;
        border: 1px dotted #ccc;
        border-radius: 15px;
        display: flex;
        align-items: center;
        justify-content: center;
        span{
          margin-left: 10px;
          font-size: 18px;
        }
      }
    }
    .el-main{
      position: relative;
      padding: 0 1vw;
      .el-divider{
        margin: 5px 0; 
      }
      .items-center{
        display: flex;
        font-size: 0.9rem;
      }
      .el-carousel {
        width: 100%; /* 轮播图的宽度 */
        height: 120px; /* 轮播图的高度 */
        margin: 3vh 0;
        border-radius: 5px;
        .carousel-image {
        position: relative;
        width: 100%; /* 图片宽度适应轮播区域 */
        height: 100%; /* 图片高度适应轮播区域 */
        object-fit: cover; /* 确保图片不会失真，同时覆盖整个轮播区域 */
        }
        .carousel-text{
          width: 100%;
          height: 100%;
          position: absolute;
          left: 50%;
          top: 0%;
          transform: translateX(-50%);
          color: #ffffff;
          // 字体居中
          text-align: center;
          padding: 20px 0;
          .title{
              font-weight: bold;
              font-size: 2rem;
              margin: 20px 0;
          }
          .timi{
              font-size: 1.1rem;
              margin: 20px 0;
          }
          .content{
            font-weight: bold;
            font-size: 1.2rem;
            margin: 20px 0;
          }
        }
      }
      .essay_title {
        font-size: 0.9rem;
        display: flex;
    }
    }
    @media screen and (max-width: 1600px) {
      margin: 0 7% ;
    }
  }
  .layout-aside{
    margin-left: 1vw;
    margin-right: 2vw;
    width: 22.5%;
    @media screen and (max-width: 1600px) {
      margin-right: 4vw;
    }
    @media screen and (max-width: 910px){
    display: none;
  }
    .cardpre {
      text-align: left;
      font-size: 1rem;
      word-wrap: break-word;
      line-height: 1.2rem;
    }
  }
}

:deep(.el-carousel__button){
  width: 5px;
  height: 5px;
  border-radius: 50%;
}
:deep(.el-aside){
  margin-left: 10px;
}


//标签云
.custom-tagcloud{
  display: flex;
  justify-content: center;
}





</style>