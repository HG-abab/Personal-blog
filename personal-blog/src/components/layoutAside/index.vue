<template>
  <div v-slide-in class="Avatar">
    <div class="elavatar">
      <div style="display: flex;justify-content: center;align-items: center;">
        <el-avatar :src="userAvatar" :size="80" />
      </div>
      <div class="username">
        Moyu
      </div>
      <div class="truncate ">
        摸鱼摸鱼摸鱼,说三遍
      </div>
    </div>
    <div class="quantity">
      <div>
        <span>8</span>
        <span>文章数</span>
      </div>
      <el-divider direction="vertical" style="font-size: 2.4rem;" />
      <div>
        <span>8</span>
        <span>分类数</span>
      </div>
      <el-divider direction="vertical" style="font-size: 2.4rem" />
      <div>
        <span>8</span>
        <span>评论数</span>
      </div>
    </div>
    <div class="icon">
      <a href="https://github.com/HG-abab">
        <div>
          <SvgIcon name="github_icon" width="24px" height="24px" />
        </div>
      </a>
      <a href="https://gitee.com/abaabab123/personal-blog">
        <div>
          <SvgIcon name="gitee_icon" width="24px" height="24px" />
        </div>
      </a>
    </div>
  </div>
</template>

<script setup>
  import { ref } from "vue";
  import Avatar from '@/assets/imgs/Avatar.jpg';


  const userAvatar = ref(Avatar);
</script>

<style lang="scss" scoped>
.Avatar{
  width: 99%;
  height: 300px;
  background-color: var(--mao-bg-content);
  border: 1px solid var(--el-border-color);
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, .1);
  .elavatar{
    border-radius: 5px;
    height: 190px;
     background: url('@/assets/imgs/image.png') no-repeat center ; 
     background-size: cover;
    .el-avatar{
      margin: 25px 0 10px 0;
      transition: all 1s;
      transform: rotate(0deg);
      &:hover{
        transition: all 1s;
        transform: rotate(360deg);
      }
    }
    .username{
      font-size: 1.2rem;
      display: flex;
      justify-content: center;
      color: #ffffff;
    }
    .truncate{
      display: flex;
      justify-content: center;
      color: rgb(181, 182, 183);
      font-size: 0.8rem;
      padding: 1.2vw 0;
    }
  }
}
.quantity{
    display: flex;
    justify-content: space-around;
    width: 100%;
    margin-top: 0.6rem;
    div {
      display: flex;
      align-items: center;
      flex-direction: column;

      span {
        font-size:1.2rem;
      }

      span:last-child {
        margin-top: 1.3vh;
        color: grey;
        font-size:1.1rem;
      }
    }
  }

  .icon {
    display: flex;
    padding: 15px 5px;
    div{
      padding-right: 10px;
    }
  }
</style>