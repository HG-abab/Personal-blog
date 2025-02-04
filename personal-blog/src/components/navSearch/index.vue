<template>
  <div class="content_container">
    <!-- 搜索框内容 -->
    <div class="search">
      <el-input v-model="input" @focus="showSearch = false" @blur="showSearch = true" size="large" style="font-size:16px;" :placeholder="placeholder">
        <template #prefix>
          <svgIcon name="search" height="24" width="24" />
        </template>
        <template #suffix>
          <div class="custom-style">
            <el-segmented block v-model="optionsValue" @change="handleChange" :options="options" size="small" />
          </div>
        </template>
      </el-input>
    </div>
    <template v-if="showSearch">
      <div class="search_history">
        <!-- 搜索 -->
        <div class="header_history">
          <div>
            搜索历史
          </div>
          <div class="event_history">
            <div class="icon">
              <el-icon>
                <Delete />
              </el-icon>
            </div>
            <span>清除记录</span>
          </div>
        </div>
      </div>
      <!-- 历史记录 -->
      <div>
        <el-check-tag checked type="primary" style="margin-left: 0.75rem; margin-top: 0.4rem ;margin-bottom: 0.4rem ;width: 35px;height: 35px;display: flex;align-items: center;justify-content: center; font-size:0.9rem; border-radius:5px;">奶龙</el-check-tag>
      </div>
      <div class="header_history" style="margin-bottom: 0.4rem;">
        <div>热门推荐</div>
        <div class="event_history">
          <div class="icon">
            <el-icon>
              <Loading />
            </el-icon>
          </div>
          <span>换一换</span>
        </div>
      </div>
      <div class="recommend_container" v-for="item in 4">
        <div class="text">
          项目部署文档
        </div>
        <div class="icon">
          <div class="text">
            <svgIcon name="heat" height="20" width="20" />
            <span>1234</span>
          </div>
        </div>
      </div>
    </template>
    <template v-else>
      <div class="showSearchText"> {{showSearchText}}</div>
    </template>
  </div>
</template>

<script setup>

  import { ref } from 'vue';

  const input = ref('')
  const showSearch = ref('false')
  const optionsValue = ref('标题')
  const options = ['标题', '内容']
  const placeholder = ref('请输入搜索内容')
  const showSearchText = ref('请输入要搜索的内容')

  const handleChange = (e) => {
    if (e === '标题') {
      placeholder.value = '请输入搜索内容'
      showSearch.value = false
      showSearchText.value = '请输入要搜索的内容'
    } else {
      placeholder.value = '回车进行内容搜索'
      showSearch.value = false
      showSearchText.value = '内容搜索每分钟只能5次'
    }
  }
</script>

<style lang="scss" scoped>

  .el-segmented {
    --el-segmented-item-selected-color: var(--el-bg-color);
    --el-segmented-item-selected-bg-color: #ff8787;
    --el-border-radius-base: 16px;
    font-size: 0.8em;
    color: grey;
  }

  .header_history{
    display: flex;
    font-size: 1.1rem;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    font-weight: bold;
    .event_history{
      display: flex;
      align-items: center;
      .icon{
        margin-top: 2px;
      }
      span{
      margin-left: 5px;
      }
      &:hover{
        cursor: pointer;
        color: #FE2C55FF;
        transition: color 0.3s linear;
      }
    }
  }

  .recommend_container{
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 6px;
    margin: 0 5px;
    border-radius: 5px;
    font-size: 1rem;
    &:hover{
      cursor: pointer;
      color: #FE2C55FF;
      background-color: #e0e2e5;
      transition: background-color 0.3s linear;
      }
      .icon{
        font-size: 0.8rem;
        .text{
          display: flex;
          align-items: center;
        }
        span{
          margin-left: 0.4rem;
        }
      }
  }
  :deep(.el-input__wrapper){
    padding: 0 4px;
  }
  .el-check-tag{
    font-size: 8px;
    background-color: #e0e2e5;
    border-radius: 2px;
    padding: 2px 8px ;
  }
  .showSearchText{
    font-size: 8px;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 8px;
  }
</style>