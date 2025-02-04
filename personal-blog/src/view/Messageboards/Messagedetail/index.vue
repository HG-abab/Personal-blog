<template>
  <div class="content1">
    <div>
      <el-link :icon="ArrowLeftBold" @click="$router.push('/message')">回到留言列表</el-link>
      <el-divider />
    </div>
    <div class="user">
      <span><el-avatar :src="commentsList.avatar" /></span>
      <div class="detail">
        <span class="name">{{ commentsList.name }}</span>
        <span class="time">{{ commentsList.time }}</span>
      </div>
    </div>
    <div class="content">
      <MdPreview :modelValue="commentsList.content" :theme="mode" />
    </div>
    <div class="container">
      <div class="count">
        <div>
          <SvgIcon name="comments" />
          <span>{{ commentsList.Reply }}</span>
        </div>
        <div @click="likeFunc">
          <SvgIcon v-show="!like" name="like" />
          <SvgIcon v-show="like" name="like-selected" />
          <span>{{ commentsList.ThumbsUp }}</span>
        </div>
        <div @click="favoriteFunc">
          <SvgIcon v-show="!favorite" name="collection" />
          <SvgIcon v-show="favorite" name="collection-selected" />
          <span>{{ commentsList.collection }}</span>
        </div>
      </div>
    </div>
    <el-divider content-position="left" />
    <comment />
  </div>
</template>

<script setup>
  import { ref } from "vue";
  import { ArrowLeftBold } from "@element-plus/icons-vue";

  import { MdEditor, MdPreview } from 'md-editor-v3';
  import 'md-editor-v3/lib/style.css';
  import { ElMessage } from "element-plus";
  import { useColorMode } from "@vueuse/core";
  import Chance from 'chance'
  import identicon from 'identicon.js';
  import comment from "../../../components/comments/index.vue";

  const mode = useColorMode()
  const commentsList = ref({
    id: 1,
    avatar: "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAMAAADXqc3KAAADAFBMVEWvdB+We9gAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADq4qvSAAABAHRSTlP//wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKmfXxgAAAmNJREFUeNoBWAKn/QAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAAAAAAEBAQEBAQEBAAAAAAABAQEBAQEBAQAAAAABAQEBAQEBAQAAAAAAAQEBAQEBAQEAAAAAAQEBAQEBAQEAAAAAAAEBAQEBAQEBAAAAAAEBAQEBAQEBAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAANDnAYHCAM7AAAAAAElFTkSuQmCC",
    content: "放大无法",
    name: "Nathaniel Little",
    Reply: 1,
    ThumbsUp: 1,
    collection: 0,
    List: [
    ],
    time: "2024/12/26 20:52:56"
  })
  const like = ref(false)
  const favorite = ref(false)
</script>

<style lang="scss" scoped>
.content1{
  margin: 1.4rem;
  width: 100%;
  .user {
    display: flex;
    align-items: center;
    justify-content: flex-start;
  .detail {
    display: flex;
    flex-direction: column;
    margin-left: 0.4rem;
    }
  .name {
    font-size: 1em;
    margin-bottom: 0.5em;
    color: #0072ff;
    }
  .time {
    font-size: 0.75em;
    color: grey;
    }
  }
  .content {
    margin: 1rem 0;
  }
  .container{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 2rem;
  }
  .count {
    display: flex;
    margin-top: 0.5rem;
    div {
      display: flex;
      align-items: center;
      margin-right: 1rem;
      color: grey;
    }
    span {
      margin-left: 0.2rem;
    }
  }
}

:deep(.md-editor-toolbar-left) {
  flex-wrap: wrap;
}
</style>