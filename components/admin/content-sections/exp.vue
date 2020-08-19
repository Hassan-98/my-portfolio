<template>
  <section class="exp-settings">
    <div class="container">
      <h2>Experince</h2>
      <hr>
      <div class="nav">
        <button @click="openAdd"><i class="fas fa-plus-square mr-1" /> Add New Experince</button>
      </div>
      <div class="current">
        <table>
          <tbody>
            <tr>
              <th>#</th>
              <th>Name</th>
              <th>Details</th>
              <th>From</th>
              <th>To</th>
              <th>Type</th>
              <th>Edit</th>
              <th>Delete</th>
            </tr>
            <tr v-if="exps.length == 0">
              <td colspan="8">No Experinces Yet</td>
            </tr>
            <tr v-for="exp in exps" v-else :key="exp._id">
              <td>#</td>
              <td>{{ exp.name }}</td>
              <td>{{ exp.details }}</td>
              <td>{{ exp.dateFrom }}</td>
              <td>{{ exp.dateTo | dateToFormat }}</td>
              <td>{{ exp.type | typeFormat }}</td>
              <td>
                <button class="edit" @click="editExp(exp._id)">
                  <i class="fas fa-edit" />
                </button>
              </td>
              <td>
                <button class="del" @click="deleteExp(exp._id)">
                  <i class="fas fa-trash-alt" />
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="editMode">
        <button class="back" @click="back"><i class="fas fa-long-arrow-alt-left" /></button>
        <h3>Add/Edit Experince</h3>
        <label>Experince Name</label>
        <input v-model="name" type="text" placeholder="Experince Name">
        <label>Experince Date From</label>
        <input
          v-model="dateFrom"
          type="number"
          min="1900"
          max="2099"
          step="1"
          placeholder="Experince Date From"
        >
        <label>Experince Date To</label>
        <input
          v-model="dateTo"
          type="number"
          min="1900"
          max="2099"
          step="1"
          placeholder="Experince Date To"
        >
        <label>Experince Type</label>
        <select v-model="type">
          <option value="experince">Experince</option>
          <option value="education">Education</option>
        </select>
        <label>Experince Details</label>
        <textarea v-model="details" col="30" row="6" placeholder="Experince Details" />
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
         dateFrom: '',
         dateTo: '',
         type: 'experince',
         exps: []
      }
   },
  methods: {
    async reFetchAndGoBack() {
     const { data } = await this.$axios.get('/api/exps')
     this.exps = data
     this.back()
    },
    async editExp(id) {
      const { data } = await this.$axios.get(`/api/exps?id=${id}`)
      document.querySelector('.nav').style.display = 'none'
      document.querySelector('.current').style.display = 'none'
      document.querySelector('.editMode').style.display = 'block'
      document.querySelector('.saveBTN').id = id
      this.name = data.name
      this.details = data.details
      this.dateFrom = data.dateFrom
      this.dateTo = data.dateTo
      this.type = data.type
    },
    async deleteExp(id) {
      const agree = confirm("Are You Sure ?");
      if (agree) {
         const {data} = await this.$axios.delete(`/api/exps?id=${id}`)
         if (data == "Deleted") this.reFetchAndGoBack()
      }
    },
    openAdd (e) {
      document.querySelector('.nav').style.display = 'none'
      document.querySelector('.current').style.display = 'none'
      document.querySelector('.editMode').style.display = 'block'
      document.querySelector('.saveBTN').id = ''
      this.name = ''
      this.details = ''
      this.dateFrom = ''
      this.dateTo = ''
      this.type = 'experince'
    },
    async save (e) {
       e.target.textContent = 'Saving...'
       const ID = e.target.id
       const { name, details, dateFrom, dateTo, type } = this

       if (name != '' && details != '' && dateFrom != '' && type != '') {
         const DataToSend = { name, details, dateFrom, dateTo, type }
         if (ID != '') {
            var res = await this.$axios.patch(`/api/exps?id=${ID}`, DataToSend)
         } else {
            var res = await this.$axios.post('/api/exps', DataToSend)
         }
         if (res.data._id) this.reFetchAndGoBack()
         e.target.textContent = 'Save'
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
      this.name = ''
      this.details = ''
      this.dateFrom = ''
      this.dateTo = ''
      this.type = 'experince'
    }
  },
  filters: {
     dateToFormat (date) {
        if (date.length > 0) {
           return date
        }
        return '-'
     },
     typeFormat (type) {
        var firstLetter = type[0]
        var capitalized = firstLetter.toUpperCase();
        var newWord = type.replace(firstLetter, capitalized)
        return newWord
     }
  },
  async created () {
     const { data } = await this.$axios.get('/api/exps')
     this.exps = data
  }
};
</script>

<style lang="scss">
.exp-settings {
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
     input, select, textarea {
        display: block;
        margin: 0 0 20px 0;
        color: #000000;
        width: 100%;
        border-radius: 5px;
     }
     textarea {
        height: 150px;
     }
     select {
        padding: 10px 15px;
        outline: none;
        option {
           color: #000000;
        }
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
