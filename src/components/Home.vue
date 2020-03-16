<template>
  <el-container class="home-container">
    <!-- 左边侧边栏 -->
    <el-aside width="170px">
      <div class="logo_box">
        <img src="../assets/logo.png">
        <span>后台管理系统</span>
      </div>
      <el-menu background-color="#373d41" text-color="#fff" active-text-color="#409EFF" :unique-opened="true" :router="true" :default-active="$route.path">
        <!-- 一级菜单 -->
        <el-submenu :index="item.id + ''" v-for="item in menulist" :key=item.id>
          <template slot="title">
            <i :class="iconsObj[item.id]"></i>
            <span>{{item.authName}}</span>
          </template>
          <!-- 二级菜单 -->
          <el-menu-item :index="'/' + subItem.path + ''" v-for="subItem in item.children" :key="subItem.id">
            <template slot="title">
              <i class="el-icon-caret-right"></i>
              <span>{{subItem.authName}}</span>
            </template>
          </el-menu-item>
        </el-submenu>
      </el-menu>
    </el-aside>
    <el-container>
      <!-- 右侧头部区域 -->
      <el-header>
        <div>
          <span>Vue后台管理系统</span>
        </div>
        <el-button type="primary" @click="logout">退出</el-button>
      </el-header>
      <!-- 右侧内容区域 -->
      <el-main>
        <!-- 路由占位符 -->
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data () {
    return {
      // 左侧菜单数据
      menulist: [],
      iconsObj: {
        125: 'el-icon-user-solid',
        103: 'el-icon-s-claim',
        101: 'el-icon-s-goods',
        102: 'el-icon-s-order',
        145: 'el-icon-s-data'
      }
    }
  },
  created () {
    this.getMenuList()
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取所有的菜单
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
      console.log(res)
    }
  }
}
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
  .logo_box {
    color: #fff;
    display: flex;
    align-items: center;
    margin: 20px 0 5px 20px;
  }
}
.el-header {
  background-color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 15px;
}
.el-aside {
  background-color: #373d41;
  .el-menu {
    border-right: none;
  }
}
.el-main {
  background-color: #F5F5F5;
}
</style>
