<template>
  <div class="admin">
    <!-- AUTH SECTION -->
    <p v-if="warn" class="alert alert-danger">Wrong Username Or Password</p>
    <section v-if="!isAuthenticated" class="login">
      <img src="/logo.png" alt="">
      <h1>Admin Login</h1>
      <input v-model="username" type="text" placeholder="Username">
      <input v-model="password" type="password" placeholder="Password">
      <button @click="auth">Authenticate</button>
    </section>

    <!-- ADMIN PANEL VIEW -->
    <section v-else class="admin-panel">
      <i v-if="!this.$store.state.sidebar && Mobile" class="showSideBar fas fa-sliders-h" @click="showSidebar" />
      <div class="row" style="direction: rtl">
        <SIDEBAR />
        <CONTENT />
      </div>
    </section>
  </div>
</template>

<style lang="scss">
.admin {
  text-align: center;
  position: relative;
  min-height: 100vh;
  p.alert {
    width: 50%;
    margin: auto;
  }
  .showSideBar {
    position: fixed;
    top: 80px;
    right: 0;
    padding: 10px;
    background: #3E4148;
    border-radius: 5px 0 0 5px;
    box-shadow: 2px 2px 6px rgba($color: #000000, $alpha: 0.1);
    cursor: pointer;
    z-index: 9;
  }
  .login {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: #3E4148;
    padding: 100px 35px;
    width: 65%;
    margin: auto;
    border-radius: 15px;
    box-shadow: 1px 1px 5px rgba($color: #000000, $alpha: 0.1);
    img {
      width: 120px;
      height: 120px;
      background: #3A3C41;
      border-radius: 50%;
      box-shadow: inset 1px 1px 5px rgba($color: #000000, $alpha: 0.4);
    }
    h1 {
      margin: 10px 0 30px;
      text-transform: uppercase;
    }
    input {
      display: block;
      margin: 30px auto;
      width: 50%;
      color: #000;
      border-radius: 5px;
    }
    button {
      background: #3A3C41;
    }
  }
}
</style>

<script>
/* eslint-disable */
import c from 'js-cookie'
import SIDEBAR from '@/components/admin/sidebar'
import CONTENT from '@/components/admin/content'

export default {
  data() {
    return {
      isAuthenticated: false,
      warn: false,
      username: '',
      password: '',
      Mobile: false
    };
  },
  layout: 'admin',
  components: {
    SIDEBAR, CONTENT
  },
  methods: {
    open(link) {
      window.open(link, "_blank");
    },
    async auth () {
      const { username, password } = this
      const cred = {
        username, password
      }
      const {data} = await this.$axios.post('/api/auth/signin', cred)
      if (data !== 'Wrong Username Or Password') {
        this.isAuthenticated = true
        this.warn = false
        const updatedData = delete data.password
        c.set('isAuthenticated', true, { expires: new Date(new Date().getTime() + 3 * 60 * 60 * 1000) })
        c.set('loggedInUser', updatedData, { expires: new Date(new Date().getTime() + 3 * 60 * 60 * 1000) })
      } else {
        this.warn = true
      }
    },
    showSidebar () {
      this.$store.commit('showSidebar')
    }
  },
  mounted () {
    var isAuthenticated = c.get('isAuthenticated')
    if (isAuthenticated == 'true') {
      isAuthenticated = true
    } else {
      isAuthenticated = false
    }
    this.isAuthenticated = isAuthenticated

    var mob = window.matchMedia('(max-width: 767px)').matches
    this.Mobile = mob
  }
};
</script>
