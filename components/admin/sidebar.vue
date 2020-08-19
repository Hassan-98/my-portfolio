<template>
  <aside class="sidebar col-lg-2 col-md-4 col-12" :class="{hide: !this.$store.state.sidebar && Mobile}">
    <div class="divv">
      <section class="top">
        <i v-if="Mobile" class="fas fa-times-circle close" @click="hide" />
        <img src="/logo.png" alt="logo">
        <h2>Admin</h2>
      </section>
      <section class="menu">
        <ul>
          <li>
            <button class="active" @click="goTo($event, 'GeneralSetting')"><i class="fas fa-cog" /> General</button>
          </li>
          <li>
            <button @click="goTo($event, 'PortfolioSetting')"><i class="fas fa-images" /> Portfolio</button>
          </li>
          <li>
            <button @click="goTo($event, 'SkillsSetting')"><i class="fas fa-star" /> Skills</button>
          </li>
          <li>
            <button @click="goTo($event, 'CertsSetting')"><i class="fas fa-certificate" /> Certificates</button>
          </li>
          <li>
            <button @click="goTo($event, 'ClientsSetting')"><i class="fas fa-users" /> Clients</button>
          </li>
          <li>
            <button @click="goTo($event, 'StatsSetting')"><i class="fas fa-gem" /> Statstics</button>
          </li>
          <li>
            <button @click="goTo($event, 'ExpSetting')"><i class="fas fa-bookmark" /> Experience</button>
          </li>
          <li>
            <button @click="goTo($event, 'ContactSetting')"><i class="fas fa-address-card" /> Contact</button>
          </li>
          <li>
            <button @click="logout"><i class="fas fa-sign-out-alt" /> Logout</button>
          </li>
        </ul>
      </section>
    </div>
  </aside>
</template>

<script>
/* eslint-disable */
import c from 'js-cookie'
export default {
  data () {
    return {
      Mobile: false
    }
  },
  methods: {
    hide () {
      this.$store.commit('hideSidebar')
    },
    goTo (e, com) {
      document.querySelectorAll('li button').forEach(e => e.classList.remove('active'))
      
      if (e.target.tagName == 'BUTTON') {
        e.target.classList.add('active')
      } else {
        e.target.parentElement.classList.add('active')
      }

      this.$store.commit('openSection', com)

      if (this.Mobile) this.$store.commit('hideSidebar')
    },
    logout () {
        var agree = confirm('Logout ?')
        if (agree) {
          c.set('isAuthenticated', false)
          c.set('loggedInUser', null)
          location.reload()
        }
    }
  },
  created () {
    var mob = window.matchMedia('(max-width: 767px)').matches
    this.Mobile = mob
  }
}
</script>

<style lang="scss">
  .sidebar {
    background: #3E4148;
    box-shadow: 2px 2px 5px rgba($color: #000000, $alpha: 0.1);
    min-height: 100vh;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 99;
    padding: 0;
    direction: ltr;
    &.hide {
      left: -1000px;
    }
    .top {
      padding: 25px 0;
      border-bottom: 2px solid #3A3C41;
      position: relative;
      .close {
        position: absolute;
        top: 10px;
        right: 10px;
      }
      img {
        width: 100px;
        height: 100px;
        box-shadow: inset 0 0 5px rgba($color: #000000, $alpha: 0.3);
        border-radius: 50%;
      }
      h2 {
        text-transform: uppercase;
      }
    }
    .menu {
      ul {
        list-style: none;
        padding: 0;
        li {
          button {
            font-size: 18px;
            width: 100%;
            border-radius: 0;
            border: none;
            border-bottom: 1px solid #3A3C41;
            padding: 15px;
            background: #3E4148;
            box-shadow: none;
            &:hover, &.active {
              transform: none;
              background: #3A3C41;
            }
            i {
              margin-right: 5px;
            }
          }
        }
      }
    }
  }
</style>
