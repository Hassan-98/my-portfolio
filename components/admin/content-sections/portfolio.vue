<template>
  <section class="portfolio-settings">
    <div class="container">
      <h2>Portfolio</h2>
      <hr>
      <div class="nav">
        <button @click="openAdd"><i class="fas fa-plus-square mr-1" /> Add New Project</button>
      </div>
      <div class="current">
        <table>
          <tbody>
            <tr>
              <th>#</th>
              <th>thumb</th>
              <th>Title</th>
              <th>Langs</th>
              <th>Edit</th>
              <th>Delete</th>
            </tr>
            <tr v-if="portfolio.length == 0">
              <td colspan="6">No Projects Yet</td>
            </tr>
            <tr v-for="proj in portfolio" v-else :key="proj._id">
              <td>#</td>
              <td><img :src="proj.thumb"></td>
              <td>{{ proj.name }}</td>
              <td>{{ proj.langs | formatLangs }}</td>
              <td>
                <button class="edit" @click="editProject(proj._id)">
                  <i class="fas fa-edit" />
                </button>
              </td>
              <td>
                <button class="del" @click="deleteProject(proj._id)">
                  <i class="fas fa-trash-alt" />
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="editMode">
        <button class="back" @click="back"><i class="fas fa-long-arrow-alt-left" /></button>
        <h3>Add/Edit Project</h3>
        <label>Project Title</label>
        <input v-model="title" type="text" placeholder="Project Title">
        <label>Project Langs</label>
        <input v-model="langs" type="text" placeholder="Project Langs (Siprated by commas)">
        <label>Project URL</label>
        <input v-model="url" type="text" placeholder="Project URL">
        <label>Project Thumb</label>
        <input type="file" class="thumb">
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
         title: '',
         url: '',
         langs: '',
         portfolio: []
      }
   },
  methods: {
    async reFetchAndGoBack() {
     const { data } = await this.$axios.get('/api/portfolio')
     this.portfolio = data
     this.back()
    },
    async editProject(id) {
      const { data } = await this.$axios.get(`/api/portfolio?id=${id}`)
      this.title = data.name
      this.url = data.url
      this.langs = data.langs
      document.querySelector('.nav').style.display = 'none'
      document.querySelector('.current').style.display = 'none'
      document.querySelector('.editMode').style.display = 'block'
      document.querySelector('.saveBTN').id = id
    },
    async deleteProject(id) {
      const agree = confirm("Are You Sure ?");
      if (agree) {
         const res = await this.$axios.delete(`/api/portfolio?id=${id}`)
         if (res.data == "Deleted") this.reFetchAndGoBack()
      }
    },
    openAdd (e) {
      document.querySelector('.nav').style.display = 'none'
      document.querySelector('.current').style.display = 'none'
      document.querySelector('.editMode').style.display = 'block'
      this.title = ''
      this.url = ''
      this.langs = ''
      document.querySelector('.saveBTN').id = ''
    },
    async save (e) {
       e.target.textContent = 'Saving...'
       const ID = e.target.id
       const { title, langs, url } = this
       const img = document.querySelector('.thumb').files[0]
       const formdata = new FormData()
       if (title != '' && url != '', langs != '') {
         if (ID != '') {
            formdata.set('name', title)
            formdata.set('url', url)
            formdata.set('langs', langs)
            if (img) {
               formdata.append('thumb', img)
            }
            const res = await this.$axios.patch(`/api/portfolio?id=${ID}`, formdata)
            if (res.data._id) this.reFetchAndGoBack()
            e.target.textContent = 'Save'
         } else {
            formdata.set('name', title)
            formdata.set('url', url)
            formdata.set('langs', langs)
            if (!img) {
               e.target.textContent = 'Save'
               return alert('Empty Fields!')
            }
            formdata.append('thumb', img)
            const res = await this.$axios.post('/api/portfolio', formdata)
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
      document.querySelector('.thumb').value = ''
      this.title = ''
      this.url = ''
      this.langs = ''
    }
  },
  filters: {
     formatLangs (langs) {
        return langs.split(',').join(' | ')
     }
  },
  async created () {
     const { data } = await this.$axios.get('/api/portfolio')
     this.portfolio = data
  }
};
</script>

<style lang="scss">
.portfolio-settings {
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
