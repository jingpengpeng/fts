<template>
  <div class="navbar">
    <div class="left-menu">
      <template>
        <a class="left-menu-item" @click="$router.push('/project')">利盾消防检测系统</a>
      </template>
    </div>
    <div class="right-menu">
      <el-dropdown class="avatar-container right-menu-item hover-effect" trigger="click">
        <div>
          <div class="avatar-wrapper">
            <img :src="user_image+'?imageView2/1/w/80/h/80'" class="user-avatar" @error="imgErr">
          </div>
          <span class="username-item">{{ nick_name }}</span>
        </div>
        <el-dropdown-menu slot="dropdown" class="user-dropdown">
          <el-dropdown-item>
            <span style="display:block;" @click="userInfoVisible = true">信息修改</span>
          </el-dropdown-item>
          <el-dropdown-item divided>
            <span style="display:block;" @click="logout">退出登录</span>
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
      <template>
        <Screenfull class="right-menu-item hover-effect" />
      </template>
    </div>
    <el-dialog :visible.sync="userInfoVisible" :append-to-body="true" :close-on-click-modal="true" title="用户信息修改">
      <userInfo :visible.sync="userInfoVisible" />
    </el-dialog>
  </div>
</template>

<script>
import userInfo from '@/views/userinfo/password'
import { mapGetters } from 'vuex'
import Screenfull from '@/components/Screenfull'

export default {
  components: {
    Screenfull,
    userInfo
  },
  data() {
    return {
      userInfoVisible: false
    }
  },
  computed: {
    ...mapGetters({
      nick_name: 'name',
      user_image: 'user_image'
    })
  },
  methods: {
    imgErr(event) {
      const img = event.srcElement
      img.src = process.env.VUE_APP_HEAD_IMAGE_URL + '0.png?imageView2/1/w/80/h/80'
      img.onerror = null
    },
    logout() {
      this.$store.dispatch('user/logout').then(() => {
        this.$router.push(`/login?redirect=${this.$route.fullPath}`)
        location.reload() // 为了重新实例化vue-router对象 避免bug
      })
    },
    toProjects() {
      this.$router.push({ path: '/platform/projects' })
    },
    toDataPlatform() {
      this.$router.push({ path: '/dataplatform' })
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  /*顶部导航栏布局*/
  .navbar {
    position: relative;
    width: 100%;
    height: 50px;
    line-height: 50px;
    box-shadow: 0 1px 3px 0 rgba(0,0,0,.12), 0 0 3px 0 rgba(0,0,0,.04);
    z-index: 998;
    .left-menu {
      float: left;
      height: 100%;
      line-height: 50px;
      margin-left: 20px;

      &:focus {
        outline: none;
      }

      .left-menu-item {
        color: #409EFF;
        display: inline-block;
        padding: 0 16px 0 16px;
        height: 100%;
        font-size: 16px;
        vertical-align: text-bottom;

        &.hover-effect {
          cursor: pointer;
          transition: background .3s;

          &:hover {
            background: rgba(0, 0, 0, .1)
          }
        }
      }
    }
    .right-menu {
      float: right;
      margin-right: 0px;
      height: 100%;
      line-height: 50px;
      &:focus {
        outline: none;
      }
      .right-menu-item {
        display: inline-block;
        padding: 0 8px 0 8px;
        height: 100%;
        font-size: 18px;
        color: #5a5e66;
        vertical-align: text-bottom;

        &.hover-effect {
          cursor: pointer;
          transition: background .3s;

          &:hover {
            background: rgba(0, 0, 0, .1)
          }
        }
      }

      .avatar-container {
        margin-right: 0px;
        padding: 0 8px 0 8px;

        .username-item {
          display: inline-block;
          margin-left: 8px;
          height: 100%;
          font-size: 16px;
          color: #666666;
          vertical-align: text-bottom;
        }

        .avatar-wrapper {
          display: inline-block;
          margin-top: 5px;
          position: relative;

          .user-avatar {
            cursor: pointer;
            width: 40px;
            height: 40px;
            border-radius: 999px;
          }

          .el-icon-caret-bottom {
            cursor: pointer;
            position: absolute;
            right: -20px;
            top: 25px;
            font-size: 12px;
          }
        }
      }
    }
  }
</style>

