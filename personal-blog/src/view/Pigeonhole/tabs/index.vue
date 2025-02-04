<template>
  <div class="common-layout">
    <div class="header">
      <div class="img_text">
        <img :src="containerbg" alt="" class="img">
        <div class="title">
          <div class="title_text">标签页</div>
          <div class="title_text2">标签</div>
        </div>
      </div>
    </div>
    <div class="main">
      <div v-if="!isQueryArticle" style="display: flex;justify-content: center;align-items: center;">
        <div class="category_container">
          <div class="title">
            标签 博客
          </div>
          <div style="display: flex;">
            <div class="item_container" v-for="item in tagList" :key="item.id">
              <div>
                <div v-slide-in class="item" @click="$router.push(`/tabs/${item.id}`)">
                  <div class="item_text"># {{item.name}}</div>
                  <div class="item_num"><span>{{item.articles.length}}</span></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="isQueryArticle" style="display: flex;justify-content: center;align-items: center;">
        <div class="category_container">
          <div class="title">
            标签 - {{title}}
          </div>
          <div>
            <el-scrollbar>
              <div class="scrollbar-flex-content">
                <template v-for="item in category" :key="item.id">
                  <p @click="$router.push(`/tabs/${item.id}`)" class="scrollbar-item" :class="item.isActive?'active':''">
                    <span>{{item.name}}</span>
                  </p>
                </template>
              </div>
            </el-scrollbar>
          </div>
          <el-divider />
          <ArticleList :articleList="articlesList" />
        </div>
      </div>
    </div>
  </div>
  <Foter />
</template>

<script setup>
  import containerbg from '@/assets/imgs/card03.jpg';
  import { onMounted, ref, watch } from 'vue';
  import Foter from '@/components/Foter/index.vue';
  import { useRoute, useRouter } from 'vue-router';
  import Carousel1 from '../../../assets/imgs/Carousel01.png';
  import Carousel2 from '@/assets/imgs/Carousel02.png';
  import Carousel3 from '@/assets/imgs/Carousel03.png';
  import Carousel4 from '@/assets/imgs/1.png';
  import Carousel5 from '@/assets/imgs/2.png';
  import Carousel6 from '@/assets/imgs/3.png';
  import Carousel7 from '@/assets/imgs/4.png';
  import Carousel8 from '@/assets/imgs/5.png';
  import ArticleList from '../ArticleList/index.vue';


  const route = useRoute();
  const articlesList = ref();
  const isQueryArticle = ref(false);
  const title = ref('');


  // 创建一个响应式数据列表

  const tagList = ref([
    {
      id: 1,
      name: '前端',
      articles: [
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
          category: '技术',
          image: Carousel5
        },
      ]
    },
    {
      id: 2,
      name: '工具',
      articles: [
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
      ]
    }
    ,
    {
      id: 3,
      name: '编程',
      articles: [
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
      ]
    },
    {
      id: 4,
      name: '后端',
      articles: [
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
          category: '技术',
          image: Carousel8
        },
      ]
    }
  ])

  // 地址栏是否有分类id
  if (route.params.id) {
    isQueryArticle.value = true
    articlesList.value = tagList.value[Number(route.params.id) - 1].articles
    tagList.value.forEach((item) => {
      if (item.id == Number(route.params.id)) {
        item.isActive = true
        title.value = item.name
      } else {
        item.isActive = false
      }
    })
  }

  // 地址栏是否有分类id
  watch(() => route.params.id, (id) => {
    if (id) {
      isQueryArticle.value = true
      articlesList.value = tagList.value[Number(id) - 1].articles
      tagList.value.forEach((item) => {
        if (item.id == Number(id)) {
          item.isActive = true
          title.value = item.name
        } else {
          item.isActive = false
        }
      })
    } else {
      isQueryArticle.value = false
    }
  })


</script>

<style lang="scss" scoped>
.common-layout {
  .header {
    height: 50vh;
    .img_text {
      position: relative;
      .img {
        width: 100%;
        height: 50vh;
        object-fit: cover;
      }
      .title {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        text-align: center;
        font-family: 'Open Sans', sans-serif;
      }
      .title_text {
        margin: 3vh 0;
        color: white;
        font-size: 4.6rem;
        font-weight: normal;
        text-shadow: 0 1px 0 hsl(174, 5%, 80%), 0 2px 0 hsl(174, 5%, 75%),
        0 3px 0 hsl(174, 5%, 70%), 0 4px 0 hsl(174, 5%, 66%),
        0 5px 0 hsl(174, 5%, 64%), 0 6px 0 hsl(174, 5%, 62%),
        0 7px 0 hsl(174, 5%, 61%), 0 8px 0 hsl(174, 5%, 60%),
        0 0 5px rgba(0, 0, 0, 0.05), 0 1px 3px rgba(0, 0, 0, 0.2),
        0 3px 5px rgba(0, 0, 0, 0.2), 0 5px 10px rgba(0, 0, 0, 0.2),
        0 10px 10px rgba(0, 0, 0, 0.2), 0 20px 20px rgba(0, 0, 0, 0.3);
      }
        $shadow: #be1d7b;
        $red: #5e0c01;
        $green: #14bbc7;
        $yellow: #ae8b17;
        $shadow: #35262f;
      .title_text2 {
        font-size: 1.5rem;
        transform: skew(-10deg);
        text-shadow: $shadow 1px 1px, $shadow 2px 2px, $shadow 3px 3px,
        $shadow 4px 4px, $shadow 5px 5px, $shadow 6px 6px;
        &:nth-child(1) {
          color: $red;
        }
        &:nth-child(2) {
          color: $green;
        }
        &:nth-child(3) {
          color: $yellow;
        }
      }
    }
  }
  .main {
    min-height: 50vh;
     background-color: var(--mao-background-color);
    .category_container{
      width: 80%;
      margin: 15vh 0;
      background-color: var(--mao-bg-content);
      border-radius: 10px;
      box-shadow: 0 0 10px #0003;
      padding: 1rem;
      .title{
        font-size: 1.72rem;
        padding: 1rem;
        color: var(--mao-hearder-color);
        font-weight: 600;
      }
      .item_container{
        .item{
          display: flex;
          font-size: 1.2rem;
          margin: .5rem;
          padding: .5rem;
          border: 1px solid var(--el-border-color);
          border-radius: 5px;
          cursor: pointer;
          transition: all .3s;
          color: #565352;
          &:hover{
              border: 1px solid blue;
          }
          .item_text{
            display: flex;
            font-size: 1.33rem;
            font-weight: 700;
            padding: 0.6rem 1rem;
            position: relative;
          }
          .item_num{
            display: flex;
            width: 0.4rem;
            align-items: center;
            justify-content: center;
            color: #fff;
            margin: 0.3rem 0;
            font-size: .6rem;
            background-color: #555;
            padding: .1rem .5rem;
            border-radius: .7em;
          }
        }
      }
    }
  }
}
.scrollbar-flex-content{
  display: flex;
  white-space: nowrap; /* 防止内容换行 */
  .scrollbar-item {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 1.5em;
    margin: 0 1em;
    padding: 0.5rem 1rem;
    text-align: center;
    border-radius: 0.6em;
    border: 1px solid var(--el-color-danger-light-7);
    background: var(--el-color-danger-light-9);
    color: var(--el-color-danger);
    &:hover {
      cursor: pointer;
      color: grey;
      background: var(--el-color-danger-light-7);
    }
  }
}

</style>
