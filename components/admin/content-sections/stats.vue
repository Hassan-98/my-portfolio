<template>
  <section class="stats-settings">
    <div class="container">
      <h2>Statstics</h2>
      <hr>
      <div class="nav">
        <button @click="openAdd"><i class="fas fa-plus-square mr-1" /> Add New Statstic</button>
      </div>
      <div class="current">
        <table>
          <tbody>
            <tr>
              <th>#</th>
              <th>Name</th>
              <th>Number</th>
              <th>Picture</th>
              <th>Update</th>
              <th>Delete</th>
            </tr>
            <tr v-if="stats.length == 0">
              <td colspan="6">No Statstics Yet</td>
            </tr>
            <tr v-for="stat in stats" v-else :key="stat._id">
              <td>#</td>
              <td>{{ stat.name }}</td>
              <td>{{ stat.number }}</td>
              <td><img :src="stat.picture"></td>
              <td>
                <button class="edit" @click="editStat(stat._id)">
                  <i class="fas fa-edit" />
                </button>
              </td>
              <td>
                <button class="del" @click="deleteStat(stat._id)">
                  <i class="fas fa-trash-alt" />
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="editMode">
        <button class="back" @click="back"><i class="fas fa-long-arrow-alt-left" /></button>
        <h3>Add/Edit Statstic</h3>
        <label>Statstic Name</label>
        <input v-model="name" type="text" placeholder="Statstic Name">
        <label>Statstic Number</label>
        <input v-model="number" type="text" placeholder="Statstic Number">
        <label>Picture</label>
        <input type="file" class="pic">
        <button class="saveBTN" @click="save">Save</button>
      </div>
    </div>
  </section>
</template>

<script>
/* eslint-disable */
export default {
   data () {
      return {
         name: '',
         number: '',
         stats: []
      }
   },
  methods: {
    async reFetchAndGoBack() {
     const { data } = await this.$axios.get('/api/stats')
     this.stats = data
     this.back()
    },
    async editStat(id) {
      const { data } = await this.$axios.get(`/api/stats?id=${id}`)
      document.querySelector('.nav').style.display = 'none'
      document.querySelector('.current').style.display = 'none'
      document.querySelector('.editMode').style.display = 'block'
      document.querySelector('.saveBTN').id = id
      this.number = data.number
      this.name = data.name
    },
    async deleteStat(id) {
      const agree = confirm("Are You Sure ?");
      if (agree) {
         const {data} = await this.$axios.delete(`/api/stats?id=${id}`)
         if (data == "Deleted") this.reFetchAndGoBack()
      }
    },
    openAdd (e) {
      document.querySelector('.nav').style.display = 'none'
      document.querySelector('.current').style.display = 'none'
      document.querySelector('.editMode').style.display = 'block'
      document.querySelector('.saveBTN').id = ''
      document.querySelector('.pic').value = ''
      this.name = ''
      this.number = ''
    },
    async save (e) {
       e.target.textContent = 'Saving...'
       const ID = e.target.id
       const { name, number } = this
       const img = document.querySelector('.pic').files[0]
       const formdata = new FormData()
       if (name != '' && number != '') {
         if (ID != '') {
            formdata.set('name', name)
            formdata.set('number', number)
            if (img) {
               formdata.append('picture', img)
            }
            const res = await this.$axios.patch(`/api/stats?id=${ID}`, formdata)
            if (res.data._id) this.reFetchAndGoBack()
            e.target.textContent = 'Save'
         } else {
            formdata.set('name', name)
            formdata.set('number', number)
            if (!img) {
               e.target.textContent = 'Save'
               return alert('Empty Fields!')
            }
            formdata.append('picture', img)
            const res = await this.$axios.post('/api/stats', formdata)
            if (res.data._id) this.reFetchAndGoBack()
            e.target.textContent = 'Save'
         }
       } else {
          e.target.textContent = 'Save'
          alert('Empty Fields!')
       }
    },
    back () {
      document.querySelector('.nav').style.display = 'block'
      document.querySelector('.current').style.display = 'block'
      document.querySelector('.editMode').style.display = 'none'
      document.querySelector('.saveBTN').id = ''
      document.querySelector('.pic').value = ''
      this.name = ''
      this.number = ''
    }
  },
  async created () {
     const { data } = await this.$axios.get('/api/stats')
     this.stats = data
  }
};
</script>

<style lang="scss">
.stats-settings {
  padding: 15px 25px;
  @include xs {
    padding: 5px 15px;
  }
  .nav {
    margin-bottom: 20px;
    button {
      border-radius: 10px;
      background: green;
    }
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
  .editMode {
     display: none;
     h3 {
        margin: 20px 0;
        background: rgba($color: #000000, $alpha: 0.3);
        padding: 15px 20px;
        border-radius: 15px;
     }
     input {
        display: block;
        margin: 0 0 20px 0;
        color: #000000;
        width: 100%;
        border-radius: 5px;
     }
     button {
        background: #3E4148;
        &.back {
           padding: 3px 25px 0;
           i {
            font-size: 28px;
           }
        }
     }
  }
}
</style>
