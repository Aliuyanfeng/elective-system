<template>
  <div id="app">
    <el-menu
      default-active="/main/home"
      class="el-menu-demo"
      mode="horizontal"
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b"
      router
    >
      <el-menu-item>
        <h1>高校选课系统</h1>
      </el-menu-item>
      <!-- <el-menu-item :index="{ path: '/main/home', query: { user: this.user } }"
        >首页</el-menu-item
      > -->

      <router-link
        tag="el-menu-item"
        :to="{ path: '/main/home', query: { user: this.user } }"
        >首页</router-link
      >

      <el-menu-item index="/main/news">动态</el-menu-item>
      <el-menu-item index="/main/schedule">课表</el-menu-item>
      <el-menu-item index="/main/record">选课记录</el-menu-item>
      <el-submenu index="/main/classifyAnalyse">
        <template slot="title">图表分析</template>
        <el-menu-item index="/main/classifyAnalyse">分类分析</el-menu-item>
        <el-menu-item index="/main/moduleAnalyse">模块分析</el-menu-item>
      </el-submenu>

      <el-row class="userInfo">
        <el-dropdown>
          <el-button type="primary">
            {{ user }}<i class="el-icon-arrow-down el-icon--right"></i>
          </el-button>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>姓名：{{student.username}}</el-dropdown-item>
            <el-dropdown-item>班级：{{student.class}}</el-dropdown-item>
            <el-dropdown-item>专业：{{student.major}}</el-dropdown-item>
            <el-dropdown-item>学院：{{student.institute}}</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
        <el-button type="primary" @click="logout">退出</el-button>
      </el-row>
    </el-menu>
    <div style="overflow: hidden">
      <Aside style="float: left" v-if="isAsideAlive"></Aside>
      <router-view style="float: right; width: 75%" v-if="isRouterAlive">
      </router-view>
    </div>
    <Footer class="footer"></Footer>
  </div>
</template>

<script>
import Aside from "@/components/public/aside.vue";
import Footer from "@/components/public/footer.vue";
export default {
  provide() {
    return {
      reload: this.reload,
      areload: this.areload,
    };
  },
  data() {
    return {
      ip: "",
      user: this.$route.query.user,
      student: {},
      isRouterAlive: true,
      isAsideAlive: true,
    };
  },
  methods: {
    reload() {
      this.isRouterAlive = false;
      this.$nextTick(function () {
        this.isRouterAlive = true;
      });
    },
    areload() {
      this.isAsideAlive = false;
      this.$nextTick(function () {
        this.isAsideAlive = true;
      });
    },
    logout() {
      sessionStorage.removeItem("studentToken");
      window.location.href = "/";
    },
  },
  created() {
    // console.log(returnCitySN["cip"] + "," + returnCitySN["cname"]);
    this.$http.get("/api/getStudentInfo/" + this.user).then((res) => {
      if (res.data.status === 0) {
        this.student = res.data.student;
      }
    });
  },
  components: {
    Footer,
    Aside,
  },
};
</script>

<style lang="less" scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  position: relative;
  overflow: hidden;
  .el-menu-item {
    h1 {
      margin: 0;
      line-height: 60px;
    }
  }
  .userInfo {
    line-height: 60px;
    width: 260px;
    position: absolute;
    right: 0;
  }
}
</style>
