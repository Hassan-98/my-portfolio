<template>
  <section class="contact-settings">
    <div class="container">
      <h2>Contact Form</h2>
      <hr>
      <div class="current">
        <table>
          <tbody>
            <tr>
              <th>Name</th>
              <th>Email</th>
              <th>Message</th>
              <th>Date</th>
              <th>Delete</th>
            </tr>
            <tr v-if="msgs.length == 0">
              <td colspan="5">No Messages Yet</td>
            </tr>
            <tr v-for="msg in msgs" v-else :key="msg._id">
              <td>{{ msg.fullName }}</td>
              <td>{{ msg.email }}</td>
              <td>{{ msg.message }}</td>
              <td>{{ msg.date | formatDate }}</td>
              <td>
                <button class="del" @click="deleteMsg(msg._id)">
                  <i class="fas fa-trash-alt" />
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </section>
</template>

<script>
/* eslint-disable */
export default {
   data () {
      return {
         msgs: []
      }
   },
  methods: {
    async FetchData() {
     const { data } = await this.$axios.get('/api/contact')
     this.msgs = data
    },
    async deleteMsg(id) {
      const agree = confirm("Are You Sure ?");
      if (agree) {
         const {data} = await this.$axios.delete(`/api/contact?id=${id}`)
         if (data == "Deleted") this.FetchData()
      }
    }
  },
  filters: {
     formatDate (date) {
        return new Date(date).toLocaleString()
     }
  },
  async created () {
     this.FetchData()
  }
};
</script>

<style lang="scss">
.contact-settings {
  padding: 15px 25px;
  @include xs {
    padding: 5px 15px;
  }
  .current {
    overflow-x: scroll;
    table {
      width: 100%;
      th {
        text-transform: uppercase;
      }
      td,
      th {
        text-align: center;
        img {
          width: 80px;
          height: 80px;
        }
        button {
          border: 0;
          box-shadow: 0;
          border-radius: 50%;
          padding: 0;
          width: 50px;
          height: 50px;
          i {
            font-size: 20px;
          }
          &.del {
            background: red;
          }
          &.edit {
            background: green;
          }
        }
      }
    }
  }
}
</style>
