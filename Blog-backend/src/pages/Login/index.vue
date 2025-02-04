<script setup>
  import { useColorMode } from '@vueuse/core'
  import pageBubble from '@/utils/pageBubble'
  import { onBeforeUnmount, onMounted, reactive, ref } from "vue";
  import { notification } from 'ant-design-vue';
  import {
    LockOutlined,
    UserOutlined
  } from '@ant-design/icons-vue';
  import { useRouter } from 'vue-router'

  const mode = useColorMode()
  const bubbleCanvas = ref()
  const activeKey = ref('1')
  const formRef = ref();
  const submitLoading = ref(false)
  const router = useRouter()

  const formState = reactive({
    username: '',
    password: undefined,
  })

  const rules = {
    username: [
      {
        required: true,
        message: '请输入2-10位数字或字母',
        trigger: 'blur',
      },
    ],
    password: [
      {
        required: true,
        message: '请输入4-20位数字或字母',
        trigger: 'blur',
      },
    ],
  }

  const modeToggle = () => {
    mode.value = mode.value === 'dark' ? 'light' : 'dark'
  }

  // 提交逻辑
  const submit = () => {
    formRef.value
      .validate()
      .then(() => {
        submitLoading.value = true
        notification.success({
          message: '登录成功',
          description: '欢迎回来！',
          duration: 3,
        })
        router.push('/')
      })
      .catch((error) => {
        notification.error({
          message: `登录失败`,
          description: '请联系管理员',
          duration: 3,
        })
      });

    submitLoading.value = false
  }

  onMounted(() => {
    pageBubble.init(bubbleCanvas.value)
  })

  onBeforeUnmount(() => {
    pageBubble.removeListeners()
  })
</script>

<template>
  <div class="login-container">
    <div class="canvas">
      <canvas ref="bubbleCanvas" />
    </div>
    <div class="login-content">
      <!-- 登录头部 -->
      <div class="login-content-top">
        <div class="top-left">
          <span class="title">moyu-blog</span>
          <span class="subtitle">欢迎进入个人博客后台</span>
        </div>
        <div class="top-right" @click="modeToggle">
          <div v-if="mode === 'light'">
            <SvgIcon name="sun" color="#a8abb2" width="20px" height="20px"></SvgIcon>
          </div>
          <div v-if="mode === 'dark'">
            <SvgIcon name="moon" color="#a8abb2" width="20px" height="20px"></SvgIcon>
          </div>
        </div>
      </div>
      <el-divider class="divider" />
      <!-- 登录主体 -->
      <div class="login-content-booton">
        <!-- 登录框左侧 -->
        <div class="login-content-booton-left">
          <div class="img">
            <img src="@/assets/images/login-left.png">
          </div>
        </div>
        <el-divider direction="vertical" class="divider-vertical" />
        <div class="login-content-booton-right">
          <div class="right-title">欢迎登入后台系统</div>
          <a-form ref="formRef" :model="formState" :rules="rules" class="login-form">
            <a-tabs v-model:activeKey="activeKey" centered>
              <a-tab-pane key="1" tab="账号密码登录" />
            </a-tabs>
            <a-form-item name="username">
              <a-input style="z-index:181" placeholder="请输入用户名" allowClear v-model:value="formState.username">
                <template #prefix>
                  <UserOutlined />
                </template>
              </a-input>
            </a-form-item>
            <a-form-item name="password">
              <a-input-password placeholder="请输入密码" allowClear style="z-index:181" v-model:value="formState.password">
                <template #prefix>
                  <LockOutlined />
                </template>
              </a-input-password>
            </a-form-item>
            <a-button style="z-index:180" type="primary" block :loading="submitLoading" size="large" @click="submit">
              登录
            </a-button>
          </a-form>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.login-container{
  display: flex;
  flex-direction: column;
  height: 100vh;
  overflow: auto;
  background: var(--bg-color-container);
  display: flex;
  justify-content: center;
  align-items: center;
  .login-content{
    width: 51%;
    height: 55%;
    border-radius: .25rem;
    box-shadow: var(--c-shadow);
    overflow: hidden;
    .login-content-top{
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin: 1rem 0;
      .top-left{
        margin: 0 1rem;
        .title{
          margin-right: 1rem;
          font-size: 1.5rem;
          font-weight: 600;
          color: var(--text-color);
        }
        .subtitle{
          font-size: .8rem;
          color: var(--text-color-1);
        }
      }
      .top-right{
        z-index: 999;
        margin: 0 1rem;
        // 鼠标
        cursor: pointer;
      }
    }
    .login-content-booton{
      width: 100%;
      height: 100%;
      display: flex;
      .login-content-booton-left{
        height: 100%;
        background-color:var(--bg-color-container);
        width: 67%;
        .img{
          display: flex;
          justify-content: center;
          margin: 10% 0 ;
          img{
            width: 60%;
            height: 60%;
            object-fit: cover;
          }
        }
      }
      .login-content-booton-right{
        width: 33%;
        .right-title{
          display: flex;
          justify-content: center;
          align-items: center;
          margin-top: 6rem;
          margin-bottom: 1.5rem;
          font-size: 1.3rem;
          color: var( --text-color);
        }
      }
    }
  }
}

.divider{
  margin: 0;
}
:deep(.el-divider){
  background-color: var(--border-color) ;
  border-top: 1px var(--border-color) ;
}
.divider-vertical{
  margin: 0;
  height: 100%;
  background-color: var(--border-color) ;
  border-left: 1px var(--border-color) ;
}
.canvas{
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 10;
}

.login-form{
  margin: 0 2rem;
}


</style>
