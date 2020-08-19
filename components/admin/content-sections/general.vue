<template>
  <section class="general-settings">
    <div class="container">
      <h2>General</h2>
      <hr>
      <div class="security">
        <div class="inputBox">
          <label>Change Username</label>
          <input v-model="user.username" type="text" placeholder="New Username">
          <button @click="changeUsername(user._id)">Save</button>
        </div>
        <div class="inputBox">
          <label>Change Password</label>
          <input v-model="user.oldPass" type="password" placeholder="Current Password">
          <input v-model="user.newPass" type="password" placeholder="New Password">
          <button @click="changePassword(user._id)">Save</button>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import c from 'js-cookie'
/* eslint-disable */
export default {
  data () {
    return {
      user: {
        username: '',
        oldPass: '',
        newPass: ''
      }
    }
  },
  methods: {
    async changeUsername (id) {
      const {data} = await this.$axios.patch(`/api/auth/updateAcc?id=${id}`, {username: this.user.username})
      if (data != 'Wrong Password') {
        c.set('loggedInUser', data, { expires: new Date(new Date().getTime() + 3 * 60 * 60 * 1000) })
        alert('Changed Successfully')
        location.reload()
      }
    },
    async changePassword (id) {
      const {oldPass, newPass} = this.user
      const {data} = await this.$axios.patch(`/api/auth/updateAcc?id=${id}`, {oldPass, newPass})
      if (data != 'Wrong Password') {
        alert('Changed Successfully')
        location.reload()
      } else {
        alert(data)
      }
    }
  },
  created () {
    const user = c.get('loggedInUser')
    this.user = JSON.parse(user)
  }
}
</script>

<style lang="scss">
.general-settings {
  padding: 15px 25px;
  @include xs {
    padding: 5px 15px;
  }
  .inputBox {
     padding: 30px 0;
     border-bottom: 2px solid #3E4148;
     label {
        display: block;
     }
     input {
        display: block;
        width: 80%;
        margin: 15px 0;
        border-radius: 5px;
        color: #3E4148;
        &:focus {
           border-color: green;
        }
     }
     button {
        background: #3E4148;
        border-radius: 5px;
     }
  }
}
</style>
