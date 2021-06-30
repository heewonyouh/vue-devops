<template>
  <div id="app">
    <div class="headtitle"><h1>HAPPY HOUSE</h1></div>
    <div id="top_menu_wrap">
      <div id="_top_menu">
        <ul class="navi">
          <li v-if="isAuthenticated && getProfile">
            <span class="aboutme">[{{ getProfile }}님]</span> |
          </li>
          <li v-if="isAuthenticated" @click="logout">
            <span class="aboutme"> Logout</span> |
          </li>
          <li v-if="isAuthenticated" @click="aboutme">
            <span class="aboutme">자신의 정보</span> |
            <!-- <router-link to="/aboutme">자신의 정보</router-link> -->
          </li>
        </ul>
      </div>
    </div>

    <div class="search_box" v-if="isAuthenticated">
      <nav-header></nav-header>
      <!-- <nav>
          <router-link class="btn btn-primary" to="/">모든사원 보기</router-link> |
          <router-link class="btn btn-primary" to="/add">사원추가</router-link> |
          <router-link class="btn btn-primary" to="/search">이름으로 사원 찾기</router-link> |
          <router-link class="btn btn-primary" to="/searchbyid">아이디로 사원 찾기</router-link> |
          <router-link class="btn btn-primary" to="/chartcustomer">부서별 인원보기</router-link> |
          <router-link class="btn btn-primary" to="/chartpiecustomer">부서별 인원보기2</router-link>
        </nav> -->
      <!-- <a class="btn btn-primary" href="/">모든사원 보기</a> | -->
      <!-- ctrl+k+c -->
    </div>
    <div v-if="isAuthenticated">
      <router-view />
    </div>
    <div v-else>
      <login />
    </div>
  </div>
</template>

<script>
import { mapGetters, mapState } from "vuex";
import Login from "./components/Login";
import NavHeader from "./components/Header.vue";
import { AUTH_LOGOUT } from "./store/actions/auth";

export default {
  name: "app",
  components: {
    Login,
    NavHeader,
  },
  methods: {
    logout: function () {
      alert("로그아웃 합니다. 로그인 하십시오.");
      this.$store.dispatch(AUTH_LOGOUT).then(() => this.$router.push("/login"));
    },
    aboutme: function () {
      alert("본인의 정보를 보입니다. 구현하세요.~~.");
      //this.$router.push('/')
    },
  },
  computed: {
    ...mapGetters(["isAuthenticated", "isProfileLoaded", "getProfile"]),
    ...mapState({
      authLoading: (state) => state.auth.status === "loading",
      // ,uname: state => `${state.user.getProfile}`,
    }),
    loading: function () {
      return this.authStatus === "loading" && !this.isAuthenticated;
    },
  },
};
</script>
<style>
.navi {
  list-style-type: none;
}
.navi li {
  display: inline-block;
  text-decoration: none;
}
.logout {
  background-color: blue;
  color: burlywood;
}
.aboutme {
  color: blue;
  background-color: burlywood;
}
</style>
