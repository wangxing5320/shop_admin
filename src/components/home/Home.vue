<template>
  <el-container class="home-wrapper">
    <el-header>
      <el-row>
        <el-col :span="8" class="logo">
          <img src="@/assets/logo.png" alt="黑马logo">
        </el-col>
        <el-col :span="8">
          <h1 class="title">电商后台管理系统</h1>
        </el-col>
        <el-col :span="8">
          <div class="welcome">
            <span>欢迎上海前端25期星曜会员</span>
            <a href="javascript:;" @click.prevent="logout">退出</a>
          </div>
        </el-col>
      </el-row>
    </el-header>
    <el-container>
      <el-aside width="200px">
        <!--
          el-menu 表示菜单组件

            default-active 当前激活菜单的 index 值
            @open 菜单展开事件
            @close 菜单收起事件
            unique-opened 如果为true，表示只展开一个菜单

          el-sub-menu 表示一组菜单
            index 是唯一的，不能重复！！！
         -->
        <!--
            template: 用来包裹一级菜单，内部指定菜单的图标和菜单名称

            如果要给菜单添加 小图标，应该使用 template 来包裹整个内容
          -->

        <!-- 启用路由模式后，index就相当于 原来 router-link 中的to属性，用来指定导航的路径（哈希值） -->
        <!-- 可以使用 /home/users 或者 home/users -->

        <el-menu :unique-opened="true" :router="true" :default-active="$route.path.slice(1)" class="el-menu-vertical-demo" background-color="#545c64" text-color="#fff" active-text-color="#ffd04b">
          <!-- 一级菜单 -->
          <el-submenu :index=" menu1.id + '' " v-for="menu1 in menus" :key="menu1.id">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>{{ menu1.authName }}</span>
            </template>

            <!-- 二级菜单 -->
            <!-- 说明：通过数据获取到的 menu2.path 不是以 / 开头的。如果不手动加 / 前缀，将来可能会有问题 （ '/' + menu2.path ）-->
            <el-menu-item :index="menu2.path" v-for="menu2 in menu1.children" :key="menu2.id">
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{ menu2.authName }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>

        <!-- <el-menu :router="true" default-active="/home/users" class="el-menu-vertical-demo" @open="handleOpen" @close="handleClose" background-color="#545c64" text-color="#fff" active-text-color="#ffd04b">
          <el-submenu index="1">

            <template slot="title">
              <i class="el-icon-location"></i>
              <span>用户管理</span>
            </template>

            <el-menu-item index="/home/users">
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>用户列表</span>
              </template>
            </el-menu-item>
          </el-submenu>

          <el-submenu index="2">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>权限管理</span>
            </template>

            <el-menu-item index="/home/roles">
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>角色列表</span>
              </template>
            </el-menu-item>
            <el-menu-item index="/home/rights">
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>权限列表</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu> -->
      </el-aside>
      <el-main>
        <!-- 子路由出口 -->
        <router-view />
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  created() {
    this.getMenus()
  },

  data() {
    return {
      // 菜单
      menus: []
    }
  },

  methods: {
    // 退出功能
    logout() {
      // 1 弹出确认对话框
      // 2 用户点击确认
      //  2.1 跳回登录页面
      //  2.2 清除token

      this.$confirm('您是否确认退出?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
        // 点击确认按钮
        .then(() => {
          // 清除token
          localStorage.removeItem('token')
          // 跳回登录页面
          this.$router.push('/login')
        })
      // // 点击取消按钮
      // .catch(() => {
      //   this.$message({
      //     type: 'info',
      //     message: '已取消删除'
      //   })
      // })
    },

    /**
     * 获取菜单
     */
    async getMenus() {
      const res = await this.$http.get('/menus')

      const { data, meta } = res.data
      if (meta.status === 200) {
        this.menus = data
      }
    }
  }
}
</script>

<style scoped lang="less">
.home-wrapper {
  height: 100%;

  .el-header {
    padding: 0;
    background-color: #b3c1cd;
    color: #333;
    text-align: center;

    .logo {
      text-align: left;
    }

    .title {
      margin: 0;
      line-height: 60px;
      color: #fff;
      font-size: 36px;
    }

    .welcome {
      line-height: 60px;
      font-weight: bold;
      text-align: right;
      padding-right: 30px;

      a {
        color: #b07a17;
        text-decoration: none;
      }
    }
  }

  .el-aside {
    background-color: #545c64;
    color: #333;
    line-height: 200px;
  }

  .el-main {
    background-color: #eaeef1;
    color: #333;
  }

  .el-container:nth-child(5) .el-aside,
  .el-container:nth-child(6) .el-aside {
    line-height: 260px;
  }

  .el-container:nth-child(7) .el-aside {
    line-height: 320px;
  }
}
</style>
