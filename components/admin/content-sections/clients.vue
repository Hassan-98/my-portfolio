<template>
  <section class="clients-settings">
    <div class="container">
      <h2>Clients</h2>
      <hr>
      <div class="nav">
        <button @click="openAdd"><i class="fas fa-plus-square mr-1" /> Add New Client</button>
      </div>
      <div class="current">
        <table>
          <tbody>
            <tr>
              <th>#</th>
              <th>Name</th>
              <th>Details</th>
              <th>Picture</th>
              <th>Update</th>
              <th>Delete</th>
            </tr>
            <tr v-if="clients.length == 0">
              <td colspan="6">No Clients Yet</td>
            </tr>
            <tr v-for="client in clients" v-else :key="client._id">
              <td>#</td>
              <td>{{ client.name }}</td>
              <td>{{ client.details }}</td>
              <td><img :src="client.picture"></td>
              <td>
                <button class="edit" @click="editClient(client._id)">
                  <i class="fas fa-edit" />
                </button>
              </td>
              <td>
                <button class="del" @click="deleteClient(client._id)">
                  <i class="fas fa-trash-alt" />
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="editMode">
        <button class="back" @click="back"><i class="fas fa-long-arrow-alt-left" /></button>
        <h3>Add/Edit Client</h3>
        <label>Client Name</label>
        <input v-model="name" type="text" placeholder="Client Name">
        <label>Client Details</label>
        <input v-model="details" type="text" placeholder="Client Details">
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
         details: '',
         clients: []
      }
   },
  methods: {
    async reFetchAndGoBack() {
     const { data } = await this.$axios.get('/api/clients')
     this.clients = data
     this.back()
    },
    async editClient(id) {
      const { data } = await this.$axios.get(`/api/clients?id=${id}`)
      document.querySelector('.nav').style.display = 'none'
      document.querySelector('.current').style.display = 'none'
      document.querySelector('.editMode').style.display = 'block'
      document.querySelector('.saveBTN').id = id
      this.details = data.details
      this.name = data.name
    },
    async deleteClient(id) {
      const agree = confirm("Are You Sure ?");
      if (agree) {
         const {data} = await this.$axios.delete(`/api/clients?id=${id}`)
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
      this.details = ''
    },
    async save (e) {
       e.target.textContent = 'Saving...'
       const ID = e.target.id
       const { name, details } = this
       const img = document.querySelector('.pic').files[0]
       const formdata = new FormData()
       if (name != '' && details != '') {
         if (ID != '') {
            formdata.set('name', name)
            formdata.set('details', details)
            if (img) {
               formdata.append('picture', img)
            }
            const res = await this.$axios.patch(`/api/clients?id=${ID}`, formdata)
            if (res.data._id) this.reFetchAndGoBack()
            e.target.textContent = 'Save'
         } else {
            formdata.set('name', name)
            formdata.set('details', details)
            if (!img) {
               e.target.textContent = 'Save'
               return alert('Empty Fields!')
            }
            formdata.append('picture', img)
            const res = await this.$axios.post('/api/clients', formdata)
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
      this.details = ''
    }
  },
  async created () {
     const { data } = await this.$axios.get('/api/clients')
     this.clients = data
  }
};
</script>

<style lang="scss">
.clients-settings {
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
