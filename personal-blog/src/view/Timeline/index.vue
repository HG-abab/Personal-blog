<template>
  <div class="common-layout">
    <div class="header">
      <div class="img_text">
        <img :src="containerbg" alt="" class="img">
        <div class="title">
          <div class="title_text">时间轴</div>
          <div class="title_text2">timeline</div>
        </div>
      </div>
    </div>
    <div class="main">
      <div v-slide-in class="category_container" ref="timebgRef">
        <template v-for="(item,year) in items" :key="item.id">
          <div class="year">--{{year}}--</div>
          <div class="item" v-for="i in item" :data-text="i.published" :key="i.id" @click="$router.push(`/article/${i.id}`)">
            <div class="content">
              <img class="img" :src="i.image" />
              <h2 class="content-title">{{ i.title }}</h2>
              <p class="content-desc">{{ i.content }}</p>
            </div>
          </div>
        </template>
      </div>
    </div>
  </div>
  <Foter />
</template>

<script setup>
  import containerbg from '@/assets/imgs/card03.jpg';
  import { computed, nextTick, onMounted, onUnmounted, ref } from 'vue';
  import Carousel1 from '@/assets/imgs/Carousel01.png';
  import Carousel2 from '@/assets/imgs/Carousel02.png';
  import Carousel3 from '@/assets/imgs/Carousel03.png';
  import Carousel4 from '@/assets/imgs/1.png';
  import Carousel5 from '@/assets/imgs/2.png';
  import Carousel6 from '@/assets/imgs/3.png';
  import Carousel7 from '@/assets/imgs/4.png';
  import Carousel8 from '@/assets/imgs/5.png';

  const timebgRef = ref(null);

  // 模拟数据
  const timilinList = ref([
    {
      id: 1,
      title: 'Vue3响应式系统解析',
      reading: 123,
      comments: 25,
      likes: 42,
      collections: 78,
      content: '深入理解Vue3响应式系统及其实现原理，我们将探讨Composition API如何通过Proxy实现数据的响应式，以及与Vue2的响应式系统的区别。通过实例分析，让你更加熟练地掌握Vue3中的响应式编程。',
      tag: '前端',
      published: '2023-08-21 02:25:13',
      updated: '2023-08-21 02:26:54',
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
      published: '2023-08-22 03:15:23',
      updated: '2023-08-22 03:16:54',
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
  ])


  // 处理数据
  const handleData = (data) => {
    data = data.map((item) => {
      item.content = item.content.replace(/[*#>`~\-\\[\]()\s]|(\n\n)/g, '');
      item.content = item.content.substring(0, 60) + '...';
      item.published = item.published.substring(0, 10);
      item.updated = item.updated.substring(0, 10);
      return item;
    })
    // 根据 published 字段排序 且重新划分
    const groupedArticles = computed(() => {
      return data.reduce((groups, article) => {
        const year = new Date(article.published).getFullYear();
        if (!groups[year]) {
          groups[year] = [];
        }
        groups[year].push(article);
        return groups;
      }, {})
    })
    return groupedArticles.value
  }

  const items = ref([]);
  onMounted(async () => {
    items.value = handleData(timilinList.value)


    await nextTick(() => {
      const timebgR = timebgRef.value;
      const itemElements = timebgR.querySelectorAll('.item');
      const itemsArray = Array.from(itemElements);
      // // 将第一个时间轴项目激活，并设置时间轴背景图片为第一个项目的图片
      itemsArray[0].classList.add('item--active');
      timebgR.style.backgroundImage = `url(${itemsArray[0].querySelector('.img').src})`;

      // 监听鼠标滚动事件
      window.addEventListener('scroll', () => {
        const pos = window.pageYOffset;
        itemsArray.forEach((item, index) => {
          const itemTop = item.offsetTop;
          const itemHeight = item.offsetHeight + item.offsetTop;

          if (index === itemsArray.length - 2 && pos > itemTop + item.offsetHeight / 2) {
            itemsArray.forEach(item => item.classList.remove('item--active'));
            timebgR.style.backgroundImage = `url(${itemsArray[itemsArray.length - 1].querySelector('.img').src})`;
            itemsArray[itemsArray.length - 1].classList.add('item--active');
          } else if (pos <= itemHeight - 10 && pos >= itemTop) {
            timebgR.style.backgroundImage = `url(${item.querySelector('.img').src})`;
            itemsArray.forEach(item => item.classList.remove('item--active'));
            item.classList.add('item--active');
          }
        })
      })
    })
  })

  // 时间轴滑动 背景替换


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
    display: flex;
    justify-content: center;
    min-height: 50vh;
     background-color: var(--mao-background-color);
    .category_container{
      display: flex; 
      flex-direction: column;
      width: 80%;
      margin: 15vh 0;
      background-color: var(--mao-bg-content);
      border-radius: 10px;
      box-shadow: 0 0 10px #0003;
      padding: 1rem;
      position: relative;
      background-attachment: fixed;
      background-size: cover;
      &::before {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        background: rgba(99, 99, 99, 0.8);
        content: "";
      }
      .year{
        font-size: 2.2rem;
        display: flex;
        justify-content: center;
        background-color: #fff;
        position: sticky;
        top: 5rem;
        font-weight: 700;
        margin: 4rem 20%;
        padding: 0.5rem 0;
        color: gray;
        border-radius: 0.6em;
        opacity: 0.8;
      }
      .item {
        padding: 40px 0;
        opacity: .3;
        filter: blur(2px);
        transition: .5s;
        box-sizing: border-box;
        width: calc(30% - 2rem);
        display: flex;
        position: relative;
        transform: translateY(-80px);
        cursor: pointer;
        .content{
          position: relative;
          img{
            max-width: 100%;
            box-shadow: 0 10px 15px rgba(0, 0, 0, 0.4);
          }
          .content-title{
            font-weight: normal;
            font-size: 1.5em;
            margin: -10px 0 0 0;
            transition: 0.4s;
            padding: 0 10px;
            box-sizing: border-box;
            color: #fff;
          }
          .content-desc{
            margin: 0;
            font-size: 15px;
            box-sizing: border-box;
            color: rgba(255, 255, 255, 0.7);
            line-height: 25px;
          }
        }
        &:nth-child(even) {
          align-self: flex-end;
          margin-right: 20%;
        }
        &:nth-child(odd) {
          margin-left: 20%;
        }
        &:nth-child(even):before {
          right: auto;
          text-align: right;
          left: calc(-100% - 56px);
          padding-left: 0;
          border-left: none;
          border-right: 2px solid rgba(255, 255, 255, 0.5);
          padding-right: 15px;
        }
      }
    }
  }
}

.item--active {
  opacity: 1 !important;
  transform: translateY(0) !important;
  filter: blur(0px) !important;
}

.item--active:before {
  top: 50% !important;
  transition: 0.3s all 0.2s !important;
  opacity: 1 !important;
}

.item--active .content-title {
  margin: -30px 0 20px 0 !important;
}

.category_container::after{
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 2px;
  height: 95%;
  background: rgba(255, 255, 255, 0.169);
}

.item:before {
  content: attr(data-text);
  letter-spacing: 3px;
  width: 100%;
  position: absolute;
  color: rgba(255, 255, 255, 0.5);
  border-left: 4px solid rgba(255, 255, 255, 0.5);
  top: 50%;
  transform: translateY(-50%);
  margin-top: -3rem;
  padding-left: 0.5rem;
  opacity: 0;
  right: calc(-100% - 56px);
  font: 900 1.5em '';
}

.category_container {
  transition: background-image 0.5s ease-in-out;
}



</style>
