<template>
  <Header />
  <div is-side-bar style=" background-color: var(--bg--article);">
    <el-container class="common-layout">
      <el-container class="main">
        <el-header class="header">
          <div class="header-img" v-lazy="articles.image">
            <div class="text">
              <div style="display: flex; justify-content: start; align-items: center;">
                <div class="title">{{articles.category}} </div>
                <div style="margin-left:2rem">#{{articles.tag}}</div>
              </div>
              <div class="h1">{{articles.title}}</div>
              <div class="unmber">
                <div class="statistics">字数统计:{{HtmlMdnumber}} </div>
              </div>
              <div style="display: flex; justify-content: start; align-items: center;">
                <div class="statistics" style="margin-right: 1rem; width:auto; height: 36px;margin-top: 1rem; padding: 0 3px;">访问量:{{articles.reading}}</div>
                <div class="statistics" style="margin-right: 1rem; width:auto; height: 36px;margin-top: 1rem; padding: 0 3px;">评论数:{{articles.comments}} </div>
                <div class="statistics" style="margin-right: 1rem; width:auto; height: 36px;margin-top: 1rem; padding: 0 3px;">点赞量:{{articles.likes}} </div>
                <div class="statistics" style="margin-right: 1rem; width:auto; height: 36px;margin-top: 1rem; padding: 0 3px;">收藏量:{{articles.collections}} </div>
              </div>
              <div style="display: flex; justify-content: start; align-items: center;">
                <div class="statistics" style="margin-right: 1rem; width:auto; height: 36px;margin-top: 1rem; padding: 0 3px;">发布:{{articles.updated}}</div>
                <div class="statistics" style="margin-right: 1rem; width:auto; height: 36px;margin-top: 1rem; padding: 0 3px;">更新:{{articles.published}} </div>
              </div>
            </div>
          </div>
        </el-header>
        <el-main class="elmain">
          <MdPreview :editorId="id" :modelValue="text" :theme="mode" :onHtmlChanged="htmlmdsize" />
        </el-main>
        <el-footer class="footer">
          <el-divider border-style="dashed" content-position="left">
            <div style="display: flex;align-items: center; font-size: 1rem;">
              <svg-icon name="author_statement"></svg-icon>
              <span style="margin-left: 0.5em">声明</span>
            </div>
          </el-divider>
          <!-- 作者著作权 -->
          <div class="copyright">
            <div class="author">
              <svg-icon name="article_author" style="margin-right: 0.5em"></svg-icon>
              <strong>本文作者：Moyu </strong>
            </div>
            <div class="link">
              <svg-icon name="author_link" style="margin-right: 0.5em"></svg-icon>
              <strong>本文链接： </strong>
              <a class="copyright_a" href="">http://localhost:5173/article/1</a>
            </div>
            <div class="license">
              <div>
                <svg-icon name="author_copyright" style="margin-right: 0.5em"></svg-icon>
                <strong>版权声明： </strong>
              </div>
              <div class="license_text">
                本站所有文章除特别声明外，均采用
                &nbsp;
                <a class="copyright_a" href="" target="_blank">
                  CC BY-NC-SA 4.0
                </a> &nbsp;
                许可协议。转载请注明文章出处！
              </div>
            </div>
          </div>
          <!-- 尾部标签与点赞收藏分享 -->
          <div style="display: flex;justify-content: space-between">
            <div class="tag" style="margin: .5rem;padding: .5rem .9rem;border: 1px solid #ccc ;border-radius: 5px;">
              <div>#{{articles.tag}}</div>
            </div>
            <div class="like" style="display: flex;margin: .5rem;padding: .5rem .9rem;">
              <div style="display: flex;margin-right: 1rem">
                <SvgIcon name="like" style="margin-right: 0.5rem" />
                <span>0</span>
              </div>
              <div style="display: flex; ;margin-right: 1rem">
                <SvgIcon name="collection" style="margin-right: 0.5rem" />
                <span>1</span>
              </div>
              <div style="display: flex; ;margin-right: 1rem">
                <SvgIcon name="share" style="margin-right: 0.5rem" />
                <span>分享</span>
              </div>
            </div>
          </div>
          <!-- 打赏 -->
          <div class="tipping">
            <el-tooltip class="box-item" effect="light" placement="top">
              <template #content>
                <div class="qrCode">
                  <div>
                    <div style="text-align: center;font-size: 1rem">微信</div>
                    <el-image src="https://s2.loli.net/2024/12/19/PGVvQJ9UIm2al6O.jpg" />
                  </div>
                </div>
              </template>
              <div class="tipping-icon">
                <svg-icon name="gift" style="margin-right: 0.5rem" />
                <span>ヾ(≧▽≦*)o！</span>
              </div>
            </el-tooltip>
          </div>
          <!-- 上/下 篇文章-->
          <div class="goOn">
            <!-- 上一篇 -->
            <div>
              <div>
                <el-link>
                  上一篇：CSS Grid布局入门指南
                </el-link>
              </div>
            </div>
            <!-- 下一篇 -->
            <div>
              <div>
                <el-link>
                  下一篇：React状态管理最佳实践
                </el-link>
              </div>
            </div>
          </div>
        </el-footer>
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
        <Sidecard title="目录" prefixIcon="directory" :isScale="true" class="Mdlog">
          <Mdlog />
        </Sidecard>
      </el-aside>
    </el-container>
    <div style="margin-top: 4rem;"></div>
    <Foter />
  </div>
  <BRightLayout toTop scrollPercentage>
    <template #scroll_percentage>{{scrollPercentage}}%</template>
  </BRightLayout>
</template>

<script setup>
  import { ref, onMounted, onUnmounted } from 'vue';
  import { useRoute } from 'vue-router';
  import router from '@/router';
  import Foter from '@/components/Foter/index.vue'
  import Header from '@/components/Header/index.vue'
  import useCurrentDateTime from '../../utils/time';
  import Sidecard from '@/components/Sidecards/index.vue'
  import { ElMessage, ElMessageBox } from 'element-plus'
  import BRightLayout from '../../components/BRightLayout/index.vue'
  import Carousel1 from '@/assets/imgs/Carousel01.png';
  import Carousel2 from '@/assets/imgs/Carousel02.png';
  import Carousel3 from '@/assets/imgs/Carousel03.png';
  import Carousel4 from '@/assets/imgs/1.png';
  import Carousel5 from '@/assets/imgs/2.png';
  import Carousel6 from '@/assets/imgs/3.png';
  import Carousel7 from '@/assets/imgs/4.png';
  import Carousel8 from '@/assets/imgs/5.png';
  import Mdlog from './MdLong/index.vue';
  import { MdPreview, MdCatalog } from 'md-editor-v3';
  import 'md-editor-v3/lib/preview.css';
  import { useColorMode, useTitle } from "@vueuse/core";
  import day07Md from '../../assets/md/1.md?raw';

  // 富文本
  const id = 'preview-only';
  const text = ref(``);
  const scrollElement = document.documentElement;
  const mode = useColorMode()

  onMounted(() => {
    text.value = day07Md
  })

  // 处理文章字数
  const HtmlMdnumber = ref(0)
  const htmlmdsize = (e) => {
    const text = e.replace(/<[^>]+>/g, "").replace(/[\r\n]/g, "").replace(/[ ]/g, "").replace(/[\s+\.\!\/_,$%^*(+\"\']+|[+——！，。？、~@#￥%……&*（）]+/g, "")
    HtmlMdnumber.value = formatNumberToK(text.length);
  }


  const { date: getCurrentDate, weekday: getWeekday, time: getTime } = useCurrentDateTime()

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
  // 假装处理数据
  const route = useRoute();
  const userId = ref(route.params.id);
  const articles = ref({})
  const articlestext = ref([
    {
      id: '1',
      title: 'Vue3响应式系统解析',
      reading: 123,
      comments: 25,
      likes: 42,
      collections: 78,
      content: '深入理解Vue3响应式系统及其实现原理，我们将探讨Composition API如何通过Proxy实现数据的响应式，以及与Vue2的响应式系统的区别。通过实例分析，让你更加熟练地掌握Vue3中的响应式编程。',
      tag: '前端',
      published: '2024-08-21 02:25:13',
      updated: '2024-08-21 02:26:54',
      category: '技术',
      image: Carousel1
    },
    {
      id: '2',
      title: 'CSS Grid布局入门指南',
      reading: 150,
      comments: 30,
      likes: 50,
      collections: 90,
      content: 'CSS Grid布局入门指南： 通过实例学习CSS Grid布局的基本用法，我们将从网格容器和项目的基本概念开始，逐步讲解如何使用grid-template-columns、grid-template-rows等属性进行布局设计。掌握CSS Grid，让你轻松应对各种复杂页面布局',
      tag: '前端',
      published: '2024-08-22 03:15:23',
      updated: '2024-08-22 03:16:54',
      category: '编程',
      image: Carousel2
    },
    {
      id: '3',
      title: 'React状态管理最佳实践',
      reading: 200,
      comments: 40,
      likes: 65,
      collections: 120,
      content: 'React状态管理最佳实践： 探讨React组件状态管理的最佳策略，包括使用useState、useReducer、useContext等Hooks进行状态管理，以及如何结合Redux、MobX等第三方库实现大型应用的状态管理。让您的React项目更加高效、可维护',
      tag: '前端',
      published: '2024-08-23 04:05:32',
      updated: '2024-08-23 04:06:45',
      category: '技术',
      image: Carousel3
    },
    {
      id: '4',
      title: 'Web性能优化实用技巧',
      reading: 180,
      comments: 35,
      likes: 58,
      collections: 100,
      content: 'Web性能优化实用技巧： 提升网站性能，优化用户加载体验。我们将从网络请求、资源压缩、缓存策略、图片优化、代码分割等多个方面，详细介绍如何对Web应用进行性能优化，让你的网站在众多竞品中脱颖而出',
      tag: '优化',
      published: '2024-08-24 05:15:12',
      updated: '2024-08-24 05:17:22',
      category: '前端',
      image: Carousel4
    },
    {
      id: '5',
      title: 'Tailwind CSS快速开发指南',
      reading: 170,
      comments: 28,
      likes: 52,
      collections: 85,
      content: '使用Tailwind CSS提升前端开发效率，我们将讲解如何利用Tailwind提供的实用类快速构建响应式、美观的界面。通过实际项目案例，让你掌握Tailwind CSS的核心概念和使用技巧',
      tag: '前端',
      published: '2024-08-25 06:30:11',
      updated: '2024-08-25 06:32:40',
      category: '设计',
      image: Carousel5
    },
    {
      id: '6',
      title: 'JavaScript异步编程深度解析',
      reading: 220,
      comments: 45,
      likes: 70,
      collections: 130,
      content: '深入了解JavaScript异步编程，我们将探讨回调函数、Promise、async/await等异步编程模式。通过实例分析，让你掌握如何在项目中有效地处理异步操作，提高代码的可读性和性能。',
      tag: '前端',
      published: '2024-08-26 07:45:59',
      updated: '2024-08-26 07:47:20',
      category: '编程',
      image: Carousel6
    },
    {
      id: '7',
      title: 'Node.js入门到实战',
      reading: 240,
      comments: 50,
      likes: 75,
      collections: 140,
      content: '从Node.js的基础知识到实战项目，我们将带你一步步学习Node.js的核心概念，包括事件循环、模块系统、文件系统操作等。通过构建一个简单的RESTful API，让你掌握Node.js在后端开发中的应用。',
      tag: '后端',
      published: '2024-08-27 08:50:33',
      updated: '2024-08-27 08:52:10',
      category: '技术',
      image: Carousel7
    },
    {
      id: '8',
      title: '机器学习基础教程',
      reading: 210,
      comments: 42,
      likes: 68,
      collections: 135,
      content: '本教程旨在带你入门机器学习，我们将从基础的统计学和算法开始，逐步讲解如何使用Python进行数据预处理、模型训练和预测。通过动手实践，让你对机器学习有一个初步的认识和理解。',
      tag: '机器学习',
      published: '2024-08-28 09:55:22',
      updated: '2024-08-28 09:57:00',
      category: '教育',
      image: Carousel8
    }
  ]);
  const onarticles = ref(false)
  articlestext.value.forEach((item) => {
    if (item.id == userId.value) {
      articles.value = item;
      onarticles.value = true;
      return;
    }
  })
  if (!onarticles.value) {
    ElMessage.warning({
      message: '文章不存在',
    })
    // 跳转回去
    console.log('跳转回去')
    router.push({ path: '/' })
  }

  function formatNumberToK(num) {
    if (num < 1000) {
      return num;
    }
    else {
      return (num / 1000).toFixed(1) + 'k';
    }
  }


  // 滚动条
  const scrollPercentage = ref(0);
  const updateScrollPercentage = () => {
    const scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
    const scrollHeight = document.documentElement.scrollHeight - document.documentElement.clientHeight;
    scrollPercentage.value = Math.round((scrollTop / scrollHeight) * 100);
  };

  onMounted(() => {
    window.addEventListener('scroll', updateScrollPercentage);
  });

  onUnmounted(() => {
    window.removeEventListener('scroll', updateScrollPercentage);
  });
</script>

<style lang="scss" scoped>
.common-layout{
  margin: 0 12.8vw;
  @media screen and (max-width: 1600px) {
      margin: 0 7% ;
    }
  .main{
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

.qrCode{
  display: flex;
  width: 100%;
  height: 100%;
  div {
  display: flex;
  justify-content: center;
  flex-direction: column-reverse;
  margin: 0 0.5rem;
  }
  .el-image {
  width: 18em;
  height: 24em;
  object-fit: cover;
  }
}

// 目录
.Mdlog {
  height: auto;
  max-height: 80vh;
  overflow-y: scroll;
  top: 20px;
  position: sticky;
}

:deep(.md-editor-catalog-active){
  & > span{
    background: var(--el-color-primary-light-9);
    color: var(--el-color-primary-light-4);
    border-radius: 8px;
    font-weight: bold;
    padding: 0.5rem 0 0.5rem 0.5rem;
  }
}

:deep(.md-editor-catalog){
  span:hover{
    color: var(--el-color-primary-light-5);
  }
}

:deep(.md-editor-catalog-link){
  margin: 0;
  padding-top: 0;
  padding-bottom: 0;
  span{
    margin: 0.25rem 0.25rem;
  }
}

:deep(.md-editor-catalog-wrapper > .md-editor-catalog-link:first-of-type) {
  padding-top: 0;
}


</style>