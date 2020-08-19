<template>
  <section class="skills-settings">
    <div class="container">
      <h2>Skills</h2>
      <hr>
      <div class="nav">
        <button @click="openAdd"><i class="fas fa-plus-square mr-1" /> Add New Skill</button>
      </div>
      <div class="current">
        <table>
          <tbody>
            <tr>
              <th>#</th>
              <th>Logo</th>
              <th>Name</th>
              <th>Edit</th>
              <th>Delete</th>
            </tr>
            <tr v-if="skills.length == 0">
              <td colspan="6">No Skills Yet</td>
            </tr>
            <tr v-for="skill in skills" v-else :key="skill._id">
              <td>#</td>
              <td><img :src="skill.logo"></td>
              <td>{{ skill.name }}</td>
              <td>
                <button class="edit" @click="editSkill(skill._id)">
                  <i class="fas fa-edit" />
                </button>
              </td>
              <td>
                <button class="del" @click="deleteSkill(skill._id)">
                  <i class="fas fa-trash-alt" />
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="editMode">
        <button class="back" @click="back"><i class="fas fa-long-arrow-alt-left" /></button>
        <h3>Add/Edit Skill</h3>
        <label>Skill Name</label>
        <input v-model="name" type="text" placeholder="Skill Name">
        <label>Skill Logo</label>
        <input type="file" class="logo">
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
         skills: []
      }
   },
  methods: {
    async reFetchAndGoBack() {
     const { data } = await this.$axios.get('/api/skills')
     this.skills = data
     this.back()
    },
    async editSkill(id) {
      const { data } = await this.$axios.get(`/api/skills?id=${id}`)
      this.name = data.name
      document.querySelector('.nav').style.display = 'none'
      document.querySelector('.current').style.display = 'none'
      document.querySelector('.editMode').style.display = 'block'
      document.querySelector('.saveBTN').id = id
    },
    async deleteSkill(id) {
      const agree = confirm("Are You Sure ?");
      if (agree) {
         const res = await this.$axios.delete(`/api/skills?id=${id}`)
         if (res.data == "Deleted") this.reFetchAndGoBack()
      }
    },
    openAdd (e) {
      document.querySelector('.nav').style.display = 'none'
      document.querySelector('.current').style.display = 'none'
      document.querySelector('.editMode').style.display = 'block'
      this.name = ''
      document.querySelector('.saveBTN').id = ''
    },
    async save (e) {
       e.target.textContent = 'Saving...'
       const ID = e.target.id
       const { name } = this
       const img = document.querySelector('.logo').files[0]
       const formdata = new FormData()
       if (name != '') {
         if (ID != '') {
            formdata.set('name', name)
            if (img) {
               formdata.append('logo', img)
            }
            const res = await this.$axios.patch(`/api/skills?id=${ID}`, formdata)
            if (res.data._id) this.reFetchAndGoBack()
            e.target.textContent = 'Save'
         } else {
            formdata.set('name', name)
            if (!img) {
               e.target.textContent = 'Save'
               return alert('Empty Fields!')
            }
            formdata.append('logo', img)
            const res = await this.$axios.post('/api/skills', formdata)
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
      document.querySelector('.logo').value = ''
      this.name = ''
    }
  },
  async created () {
     const { data } = await this.$axios.get('/api/skills')
     this.skills = data
  }
};
</script>

<style lang="scss">
.skills-settings {
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
