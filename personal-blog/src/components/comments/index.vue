<template>
  <div class="comments">
    <div v-if="isShowinput">
      <div class="comments_title">
        <svg-icon name="comment" width="1.5em" height="1.5em" />
        <span>评论(1)</span>
      </div>
    </div>
    <div class="form_container">
      <textarea ref="Input" class="textarea" v-model="textinput" placeholder="留下你的精彩评论吧！" />
      <div class="btn">
        <div style="display: flex; justify-content: space-between;margin:1rem 0">
          <V3Emoji :recent="true" :theme="mode" @click-emoji="clickEmoji" :options-name="optionsName" />
          <div style="display: flex; align-items: center">
            <el-button type="info" plain size="small" @click="isPreview=!isPreview">预览</el-button>
            <el-button type="success" plain size="small" @click="addParentComment">发布</el-button>
          </div>
        </div>
        <div class="preview" v-if="isPreview">
          <MdPreview :modelValue="textinput" :theme="mode" />
        </div>
      </div>
    </div>
    <!-- 展示 -->
    <div>
      <div style="display: flex;margin-top: 1rem;border-top: 1px solid var(--el-border-color);padding-top: 1rem;" v-for="item in Comments" :key="item.id">
        <el-avatar shape="square" :size="40" :src="item.avatar" />
        <div class="comment_content">
          <div class="comment_content_header">
            <div style="display: flex;align-items: center;">
              <div>{{ item.name }}</div>
              <div v-if="item.UserId === '1'">
                <el-tag size="small">作者</el-tag>
              </div>
              <div>{{ item.time }}</div>
            </div>
            <div>
              <SvgIcon v-show="!item.isThumbsUp" name="like" style="cursor: pointer" />
              <SvgIcon v-show="item.isThumbsUp" name="like-selected" style="cursor: pointer" />
              <span style="font-size: 0.8em;color: grey">{{ item.ThumbsUp }}</span>
              <svg-icon @click="handleReply(item.id)" name="comment" style="margin:0 0.2em 0 0.5rem;cursor: pointer" />
              <span style="font-size: 0.8em;color: grey">{{ item.Reply }}</span>
            </div>
          </div>
          <!-- 父评论 -->
          <div class="comment_content_body">
            <div>
              <MdPreview :modelValue="item.content" />
            </div>
            <!-- 父回复框 -->
            <transition name="el-zoom-in-top">
              <ReplyBox :comment="item" v-show="showReplyIndex === item.id && showReply"></ReplyBox>
            </transition>
            <!-- 子评论 -->
            <template v-if="item.children && item.children.length > 0">
              <div class="ChildComment" v-for="child in item.children" :key="child.id">
                <div style="display: flex;margin-top: 1rem;border-top: 1px solid var(--el-border-color);padding-top: 1rem;">
                  <el-avatar shape="square" :size="40" :src="child.avatar" />
                  <div class="comment_content">
                    <div class="comment_content_header">
                      <div style="display: flex;align-items: center;">
                        <div>{{ child.name }}</div>
                        <div v-if="child.UserId === '1'">
                          <el-tag size="small">作者</el-tag>
                        </div>
                        <div>{{ child.time }}</div>
                      </div>
                      <div>
                        <SvgIcon v-show="!child.isThumbsUp" name="like" style="cursor: pointer" />
                        <SvgIcon v-show="child.isThumbsUp" name="like-selected" style="cursor: pointer" />
                        <span style="font-size: 0.8em;color: grey">{{ child.ThumbsUp }}</span>
                        <svg-icon @click="handleReply(child.id)" name="comment" style="margin:0 0.2em 0 0.5rem;cursor: pointer" />
                        <span style="font-size: 0.8em;color: grey">{{ child.Reply }}</span>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="children_content_body">
                  <div class="chidren_name">
                    <div class="name">
                      <span>回复 </span>
                      <span style="color:red">@{{findParentComment(Comments, child.parentId).name}}</span>:
                    </div>
                    <MdPreview :modelValue="child.content" />
                  </div>
                  <transition name="el-zoom-in-top">
                    <ReplyBox :comment="child" v-show="showReplyIndex === child.id && showReply"></ReplyBox>
                  </transition>
                </div>
              </div>
            </template>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { MdPreview } from 'md-editor-v3';
  import 'md-editor-v3/lib/preview.css';

  import { ElMessage, ElMessageBox } from "element-plus";
  import { useColorMode } from "@vueuse/core";
  import ReplyBox from './ReplyBox.vue';
  import { ref } from 'vue';

  // 表情 
  import V3Emoji from 'vue3-emoji'
  import 'vue3-emoji/dist/style.css'
  import ReplyBoxVue from './ReplyBox.vue';

  const props = defineProps({
    authorId: {
      type: Number,
      required: true
    },
    typeId: {
      type: Number,
      required: true
    },
    // 是否显示输入
    isShowinput: {
      type: Boolean,
      default: true
    },
    // 评论类型
    type: Number,
    // 点赞类型
    likeType: Number
  })
  const mode = useColorMode();
  const Input = ref('')
  const textinput = ref('')
  // 是否显示表情
  const isShowEmojis = ref(true)
  // 是否显示预览
  const isPreview = ref(false)
  const preview = ref('')
  const isShowReplyBox = ref(false)
  const showReply = ref(false)
  const showReplyIndex = ref(0)

  const optionsName = {
    "Smileys & Emotion": "笑脸&表情",
    "Food & Drink": "食物&饮料",
    "Animals & Nature": "动物&自然",
    "Travel & Places": "旅行&地点",
    "People & Body": "人物&身体",
    Objects: "物品",
    Symbols: "符号",
    Flags: "旗帜",
    Activities: "活动",
  }


  const clickEmoji = (emoji) => {
    textinput.value += emoji
  }

  // 提交评论
  const addParentComment = () => {
    if (textinput.value == '') {
      ElMessage.error("评论内容不能为空");
      return
    }
    ElMessage.success("评论成功");
    textinput.value = ''
  }

  // 模拟数据
  const Comments = [
    {
      id: 1,
      UserId: "1",
      avatar: "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAMAAADXqc3KAAADAFBMVEWvdB+We9gAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADq4qvSAAABAHRSTlP//wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKmfXxgAAAmNJREFUeNoBWAKn/QAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAAAAAAEBAQEBAQEBAAAAAAABAQEBAQEBAQAAAAABAQEBAQEBAQAAAAAAAQEBAQEBAQEAAAAAAQEBAQEBAQEAAAAAAAEBAQEBAQEBAAAAAAEBAQEBAQEBAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAANDnAYHCAM7AAAAAAElFTkSuQmCC",
      content: "This is a top-level comment.",
      name: "Alice",
      thumbsUp: 10,
      time: "2023-04-01T12:00:00Z",
      Reply: 1,
      ThumbsUp: 1,
      parentId: null,
      isShowReplyBox: false,
      children: [
        {
          id: 2,
          userId: "2",
          avatar: "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAMAAADXqc3KAAADAFBMVEWvdB+We9gAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADq4qvSAAABAHRSTlP//wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKmfXxgAAAmNJREFUeNoBWAKn/QAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQAAAAAAAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAEBAQEBAQEBAAAAAAEBAQEBAQEBAAAAAAABAQEBAQEBAQAAAAABAQEBAQEBAQAAAAAAAQEBAQEBAQEAAAAAAQEBAQEBAQEAAAAAAAEBAQEBAQEBAAAAAAEBAQEBAQEBAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAANDnAYHCAM7AAAAAAElFTkSuQmCC",
          content: "This is a reply to the top-level comment.",
          name: "Bob",
          thumbsUp: 5,
          time: "2023-04-01T12:10:00Z",
          parentId: 1,
          Reply: 1,
          ThumbsUp: 1,
          isShowReplyBox: false,
        }
      ]
    },
  ];

  // 找父级评论
  function findParentComment(comments, id) {
    for (const comment of comments) {
      if (comment.id === id) {
        return comment;
      }
      if (comment.children && comment.children.length > 0) {
        const parent = findParentComment(comment.children, id);
        if (parent) {
          return parent;
        }
      }
    }
    return null;
  }

  // 显示回复框
  const handleReply = (rootCommentId) => {
    // 解决只在 当前点击项下 显示回复框
    showReplyIndex.value = rootCommentId;
    // 控制显示隐藏
    showReply.value = !showReply.value;

    console.log(showReply.value);
    console.log(showReplyIndex.value);
    console.log(rootCommentId);
  }
</script>

<style lang="scss" scoped>
.comments {
  .comments_title {
    display: flex;
    align-items: center;
    font-size: 1.5em;
    font-weight: bold;
    margin: 2rem 0;
    span {
      margin-left: 0.5rem;
    }
  }
  .form_container {
    padding: 0.5rem 1rem;
    .textarea {
      resize: none;
      padding: 0.5rem;
      width: 100%;
      height: 7em;
      border-radius: 0.5em;
      line-height: 1.5em;
      border: 1px solid var(--el-border-color);
    }
    .preview {
      margin-top: 1rem;
      padding: 0.2rem;
      width: 100%;
      min-height: 4em;
      height: 100%;
      border-radius: 0.5em;
      line-height: 1.5em;
      border: 1px solid var(--el-border-color);
    }
    :deep(.md-editor-preview-wrapper) {
      padding: 0;
    }
  }
  .comment_content {
    display: flex;
    flex-direction: column;
    width: 100%;
    .comment_content_header {
      display: flex;
      justify-content: space-between;
      div{
        display: flex;
        margin: 0 0.2rem;
        align-items: center;
        &div:nth-child(1) {
        font-weight: bold;
        &:hover {
          color: #cb829d;
          cursor: pointer;
          transition: color 0.5s;
        }
      }
      & div:last-child {
        margin-left: 2rem;
        font-size: 0.5em;
        color: grey;
        }
      } 
    }
  }
   .comment_content_body {
    font-size: 0.9rem;
    margin: 0.5rem 0.2rem;
    padding: 0.5rem 0.7rem;
    line-height: 0.7em;
    :deep(.md-editor) {
      border-radius: 0.6rem !important;
      background-color: var(--mao-bg-comment);
    }
  }
}

.children_content_body{
  font-size: 1rem;
  margin-left: 2.6rem;
  margin-top: -1rem;
  padding: 0.5rem 0;
  line-height: 0.7em;
  :deep(.md-editor) {
    background-color: var(--mao-bg-comment);
  }
}
.chidren_name{
  min-height: 3rem;
  border-radius: 0.4rem;
  background-color: var(--mao-bg-comment);
  .name{
    font-size: 1rem;
    padding: 0 0.5rem;
    padding-top: 1rem;
    padding-bottom: 0.2rem;
  }
}

</style>