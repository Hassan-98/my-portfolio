<template>
  <div class="admin-contant col-lg-10 col-md-8 col-12">
    <nav><img src="/imgs/mypic3.png"> <span>{{ user.username }}</span></nav>
    <component :is="openedSection" />
  </div>
</template>

<script>
import c from 'js-cookie'
import GeneralSetting from './content-sections/general'
import PortfolioSetting from './content-sections/portfolio'
import CertsSetting from './content-sections/certs'
import ClientsSetting from './content-sections/clients'
import ContactSetting from './content-sections/contact'
import ExpSetting from './content-sections/exp'
import SkillsSetting from './content-sections/skills'
import StatsSetting from './content-sections/stats'

export default {
  components: {
    GeneralSetting,
    PortfolioSetting,
    CertsSetting,
    ClientsSetting,
    ContactSetting,
    ExpSetting,
    SkillsSetting,
    StatsSetting
  },
  data () {
    return {
      user: {
        username: '',
        password: ''
      }
    }
  },
  computed: {
    openedSection () {
      return this.$store.state.openedSection
    }
  },
  created () {
    const user = c.get('loggedInUser')
    this.user = JSON.parse(user)
  }
}
</script>

<style lang="scss">
.admin-contant {
  direction: ltr;
  text-align: left;
  padding: 0 10px;
  nav {
    background: #3E4148;
    padding: 15px;
    box-shadow: 2px 2px 5px rgba($color: #000000, $alpha: 0.1);
    img {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      box-shadow: inset 0px 0px 5px rgba($color: #000000, $alpha: 0.3);
      margin-right: 7px;
    }
  }
}
</style>
