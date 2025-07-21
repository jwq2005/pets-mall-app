<template>
  <div>
    <!-- 顶部导航栏 -->
    <header class="header">
      <div class="left-nav">
        <router-link
          v-if="!showLoginModal"
          @click.prevent="goBack"
          class="nav-link"
          to="#"
        >
          返回
        </router-link>
      </div>

      <router-link to="/">首页</router-link>
      <router-link to="/user/login">用户登录</router-link> |
      <router-link to="/register/user">用户注册</router-link>
      <router-link to="/login/admin">管理员登录</router-link>
    </header>

    <!-- 子路由展示 -->
    <router-view :show-login="showLogin" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      showLoginModal: false
    };
  },
  methods: {
    goBack() {
      if (this.showLoginModal) {
        this.closeLoginModal();
      } else {
        this.$router.back();
      }
    },
    showLogin() {
      this.showLoginModal = true;
    },
    closeLoginModal() {
      this.showLoginModal = false;
    },
    // 🚨 新增：同步请求 logoutAll
    logoutAllUsersBeforeUnload() {
      try {
        const xhr = new XMLHttpRequest();
        xhr.open("PUT", "http://localhost:8081/api/user/logoutAll", false); // false = 同步请求
        xhr.send();
        // 可选：清理本地存储
        localStorage.removeItem("userId");
        localStorage.removeItem("isLoggedIn");
      } catch (error) {
        console.error("网页关闭时登出失败：", error);
      }
    }
  },
  mounted() {
  window.addEventListener("unload", () => {
    navigator.sendBeacon("http://localhost:8081/api/user/logoutAll");
  });
}

};
</script>



<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px;
  background-color: #f5f5f5;
  border-bottom: 1px solid #ddd;
}

.left-nav,
.right-nav {
  display: flex;
  align-items: center;
}

.nav-link {
  margin: 0 10px;
  color: #333;
  text-decoration: none;
  font-weight: 500;
  cursor: pointer;
}

.nav-link:hover {
  color: #409EFF;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 2000;
}

.modal {
  background-color: white;
  padding: 20px 30px;
  border-radius: 10px;
  position: relative;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.close-btn {
  position: absolute;
  top: 6px;
  right: 10px;
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
}

.modal {
  background: white;
  padding: 20px 30px;
  border-radius: 10px;
  text-align: center;
  position: relative;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

.close-btn {
  position: absolute;
  right: 10px;
  top: 10px;
  border: none;
  background: transparent;
  font-size: 18px;
  cursor: pointer;
}

.login-section {
  margin-top: 10px;
}

</style>
