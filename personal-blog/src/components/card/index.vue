<template>
  <div>
    <el-card v-slide-in v-for="article in articles" :key="article.id">
      <div class="elcardbox" @click="toarticle(article.id)">
        <div class="right-img" v-if="article.id % 2 !== 0 || width < 768">
          <div class="cardimg">
            <div style="overflow: hidden;">
              <img v-lazy="article.image" alt="" class="img">
            </div>
            <div class="text">{{ article.category }}</div>
          </div>
        </div>
        <div class="main-text">
          <div class="title">{{ article.title }}</div>
          <div class="main-icon">
            <div class="flex">
              <SvgIcon name="reading" style="margin-right: 5px" />
              <span class="number">{{ article.reading }}</span>
            </div>
            <div class="flex">
              <SvgIcon name="comments" style="margin-right: 5px" />
              <span class="number">{{ article.comments }}</span>
            </div>
            <div class="flex">
              <SvgIcon name="like" style="margin-right: 5px" />
              <span class="number">{{ article.likes }}</span>
            </div>
            <div class="flex">
              <SvgIcon name="collection" style="margin-right: 5px" />
              <span class="number">{{ article.collections }}</span>
            </div>
          </div>
          <p class="main-p">{{ Filtercontent(article.content) }}</p>
          <div class="tag">
            <span>标签：</span>
            <el-tag size="small" class="eltag">{{ article.tag }}</el-tag>
          </div>
          <div class="main-time">
            <p class="time">发布于：{{ article.published }}</p>
            <p>更新于：{{ article.updated }}</p>
          </div>
        </div>
        <div class="left-img" v-if="article.id % 2 === 0  && width > 768">
          <div class="cardimg">
            <div style="overflow: hidden;">
              <img v-lazy="article.image" alt="" class="img">
            </div>
            <div class="text">{{ article.category }}</div>
          </div>
        </div>
      </div>
    </el-card>
  </div>
</template>

<script setup>
  import { onMounted, onUnmounted, ref } from 'vue';
  import Carousel1 from '@/assets/imgs/Carousel01.png';
  import Carousel2 from '@/assets/imgs/Carousel02.png';
  import Carousel3 from '@/assets/imgs/Carousel03.png';
  import Carousel4 from '@/assets/imgs/1.png';
  import Carousel5 from '@/assets/imgs/2.png';
  import Carousel6 from '@/assets/imgs/3.png';
  import Carousel7 from '@/assets/imgs/4.png';
  import Carousel8 from '@/assets/imgs/5.png';
  import router from '@/router';

  // 监听窗口宽度变化屏幕宽度
  const width = ref(window.innerWidth);

  const handleResize = () => {
    width.value = window.innerWidth;
  }

  onMounted(() => {
    window.addEventListener('resize', handleResize);
  });

  // 在组件卸载时移除事件监听器
  onUnmounted(() => {
    window.removeEventListener('resize', handleResize);
  });
  // 创建一个响应式数据列表
  const articles = ref([
    {
      id: 1,
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
      id: 2,
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
      id: 3,
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
      id: 4,
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
      id: 5,
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
      id: 6,
      title: 'JavaScript事件循环详解',
      reading: 210,
      comments: 45,
      likes: 70,
      collections: 110,
      content: '详细解析JavaScript事件循环机制，包括宏任务、微任务、任务队列等概念。通过实例分析，让你深入了解事件循环的工作原理，以及如何在实际开发中避免回调地狱，编写更高效、清晰的代码',
      tag: '编程',
      published: '2024-08-26 07:45:11',
      updated: '2024-08-26 07:47:40',
      category: '技术',
      image: Carousel6
    },
    {
      id: 7,
      title: '前端性能监控工具介绍',
      reading: 190,
      comments: 38,
      likes: 60,
      collections: 95,
      content: '介绍前端性能监控工具及其应用场景，我们将对比分析Chrome DevTools、Lighthouse、WebPageTest等工具的优势和特点。掌握这些工具，可以帮助你更好地发现和解决前端性能问题',
      tag: '工具',
      published: '2024-08-27 08:15:12',
      updated: '2024-08-27 08:17:22',
      category: '前端',
      image: Carousel7
    },
    {
      id: 8,
      title: 'Node.js中间件开发指南',
      reading: 220,
      comments: 50,
      likes: 80,
      collections: 130,
      content: '学习Node.js中间件开发和使用技巧，我们将从中间件的基本概念开始，逐步讲解如何编写自己的中间件，以及如何在实际项目中应用。通过掌握中间件开发，让你在Node.js领域更进一步',
      tag: '后端',
      published: '2024-08-28 09:30:11',
      updated: '2024-08-28 09:32:40',
      category: '后端',
      image: Carousel8
    }
  ]);
  const Filtercontent = (e) => {
    return e.substring(0, 50) + '...';
  }

  function toarticle(e) {
    router.push(`/article/${e}`);
  }
</script>
<style lang="scss" scoped>
  .el-card{
    margin: 10px 0;
    height: 220px;
    &:hover img{
      transform: scale(1.1);
      transition: transform 0.3s linear;
    }
    .elcardbox{
      display: flex;
      justify-content: space-between;
        .left-img{
        width: 50%;
        position: relative;
        .cardimg{
        height: 240px;
        .img {
          height: 240px;
          width: 100%; /* 图片宽度适应轮播区域 */
          object-fit: cover; /* 确保图片不会失真，同时覆盖整个轮播区域 */
          }
          .text{
          position: absolute;
          top: 0;
          left: 0;
          width: 50px;
          height: 40px;
          font-size:0.8rem ;
          color: #ffffff;
          display: flex;
          justify-content: center;
          align-items: center;
          backdrop-filter: blur(10px);
          background-color: rgba(255, 255, 255, 0.041);
          }
        }
      }
      .right-img{
        width: 50%;
        height: 240px;
        position: relative;
        .cardimg{
        height: 240px;
        .img {
          height: 240px;
          width: 100%; /* 图片宽度适应轮播区域 */
          object-fit: cover; /* 确保图片不会失真，同时覆盖整个轮播区域 */
          }
          .text{
          position: absolute;
          top: 0;
          left: 0;
          width: 50px;
          height: 40px;
          font-size:0.8rem ;
          color: #ffffff;
          display: flex;
          justify-content: center;
          align-items: center;
          backdrop-filter: blur(10px);
          background-color: rgba(255, 255, 255, 0.041);
          }
        }
      }
      .main-text{
        width: 50%;
        .title{
          display: flex;
          margin: 1.2vw 1.4vw;
          font-weight: bold;
          font-size: 1.8rem;
        }
        .main-icon{
          margin: 0.1vw 1vw;
          display: flex;
          font-size: 0.8rem;
          .flex{
            display: flex;
            align-items: center;
            .number{
              margin-right: 0.8vw;
            }
          }
        }
        .main-p{
          margin: 0.8vw 1vw;
          font-size: 1.2rem;
        }
        .tag{
          margin: 0.5vw 1vw;
          font-size: 1.2rem;
          display: flex;
          align-items: center;
        }
        .main-time{
          margin: 0.6vw 1vw;
          display: flex;
          font-size: 0.8rem;
          .time{
            margin-right: 0.5vw;
          }
        }
      }
    }
  }
  

  :deep(.el-card__body){
    position: relative;
    padding: 0;
    }

 @media screen and (max-width: 768px) {
    .el-card {
      flex-direction: column;
      height: auto;
      .elcardbox{
        display: block;
        .right-img {
          width: 100%;
          height: auto;
          .cardimg{
            height: 170px;
            .img {
              height: 170px;
            }
          }
        }
        .main-text {
          height: 170px;
          width: 100%;
          .title{
            display: flex;
            margin: 2vh 3vw;
            font-weight: bold;
            font-size: 1.6rem;
          }
        .main-icon{
          margin: 0.1vh 3vw;
          display: flex;
          font-size: 0.9rem;
          .flex{
            display: flex;
            align-items: center;
            .number{
              margin-right: 2vw;
            }
          }
        }
        .main-p{
          margin: 1.5vh 3vw;
          font-size: 1.2rem;
        }
        .tag{
          margin: 0.8vh 3vw;
          font-size: 1.2rem;
          display: flex;
          align-items: center;
        }
        .main-time{
          margin: 1vh 3vw;
          display: flex;
          font-size: 0.8rem;
          .time{
            margin-right: 0.8vw;
          }
        }
        }
      }
    }
 }
</style>