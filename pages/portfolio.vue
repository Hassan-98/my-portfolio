<template>
  <div class="home">
    <!-- Portfolio -->
    <section class="portfolio">
      <img src="/imgs/back/wish.svg" class="background" style="top:25%;left:35%">
      <img src="/imgs/back/space.svg" class="background" style="top:30%;left:5%">
      <img src="/imgs/back/space.svg" class="background" style="top:70%;left:90%">
      <img src="/imgs/back/space2.svg" class="background" style="top:20%;left:90%">
      <img src="/imgs/back/space2.svg" class="background" style="top:50%;left:10%">
      <img src="/imgs/back/space3.svg" class="background" style="top:75%;left:5%">
      <img src="/imgs/back/space3.svg" class="background" style="top:55%;left:85%">
      <img src="/imgs/back/space4.svg" class="background" style="top:0%;left:68%">
      <img src="/imgs/back/space4.svg" class="background" style="top:10%;left:15%">
      <img src="/imgs/back/space5.svg" class="background" style="top:76%;left:42%">
      <img src="/imgs/back/missile.svg" class="background" style="top:77%;left:20%">
      <img src="/imgs/back/missile.svg" class="background" style="top:22%;left:60%">
      <img src="/imgs/back/asteroid.svg" class="background" style="top:45%;left:45%">
      <img src="/imgs/back/asteroid.svg" class="background" style="top:42%;left:90%">
      <img src="/imgs/back/wish.svg" class="background" style="top:60%;left:32%;transform:rotate(180deg)">
      <div class="container">
        <h1>My Portfolio</h1>
        <div class="row justify-content-center">
          <div v-for="project in portfolio" :key="project._id" class="col-lg-4 col-md-6 col-12 contdiv" data-aos="fade-up" data-aos-duration="1000">
            <div class="project">
              <img :src="project.thumb">
              <div class="overlay" @click="open(project.url, true)">
                <i class="fas fa-external-link-alt" />
              </div>
              <div class="caption">
                <h4>{{ project.name }}</h4>
                <p>{{ project.langs | formatLangs }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Contact -->
    <section class="contact">
      <div class="overlay">
        <div class="container">
          <h1>Get In Touch</h1>
          <div class="row">
            <div class="col-md-5">
              <div class="info">
                <p>Feel free to get in touch with me. I am always open to discussing new projects, creative ideas or opportunities to be part of your visions.</p>
                <div class="way">
                  <h4>Mail Me</h4>
                  <i class="fas fa-at" /> <span>7assan.3li1998@gmail.com</span>
                </div>
                <div class="way">
                  <h4>Call Me</h4>
                  <i class="fas fa-phone" /> <span>01146321817</span>
                </div>
                <div class="icons">
                  <i class="fab fa-facebook-f" />
                  <i class="fab fa-twitter" />
                  <i class="fab fa-linkedin-in" />
                  <i class="fab fa-github-alt" />
                </div>
              </div>
            </div>
            <div class="col-md-7 col-12">
              <div class="form">
                <input v-model="name" type="text" placeholder="Full Name">
                <input v-model="email" type="email" placeholder="Email Address">
                <textarea v-model="message" cols="30" rows="6" placeholder="Type your message..." />
                <button>Send Message</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style lang="scss">
img.background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100px;
  height: 100px;
}
.portfolio {
  padding: 100px 0;
  position: relative;
  h1 {
    text-align: center;
    text-transform: uppercase;
    font-family: "Piedra", cursive;
    position: relative;
    margin-bottom: 100px;
    &::after {
      content: "PORTFOLIO";
      opacity: 0.1;
      font-size: 100px;
      width: 100%;
      text-align: center;
      display: block;
      position: absolute;
      top: -30px;
      left: 50%;
      transform: translateX(-50%);
      letter-spacing: 10px;
      @include md {
        font-size: 75px;
        top: -20px;
      }
      @include sm {
        font-size: 45px;
        top: -5px;
      }
    }
  }
  .contdiv {
    &:nth-of-type(even) {
      .overlay {
        transform: translateX(-600px);
      }
    }
    &:nth-of-type(odd) {
      .overlay {
        transform: translateX(600px);
      }
    }
  }
  .project {
    box-shadow: 0 10px 10px rgba($color: #000000, $alpha: 0.3);
    position: relative;
    margin-bottom: 35px;
    overflow: hidden;
    height: 500px;
    @include sm {
      height: 550px;
    }
    @include xs {
      height: 400px;
    }
    img {
      border: 5px solid #fff;
      position: relative;
      z-index: 10;
      height: 100%;
      width: 100%;
    }
    .overlay {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba($color: #3A3C41, $alpha: 0.6);
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      z-index: 11;
      i {
        font-size: 40px;
        color: #fff;
      }
    }
    .caption {
      position: absolute;
      bottom: -100px;
      left: 0;
      z-index: 9;
      width: 100%;
      box-shadow: inset 0 5px 10px rgba($color: #000000, $alpha: 0.2);
      background: rgba($color: #FFB400, $alpha: 0.85);
      border-bottom: 5px solid #aaa;
      h4 {
        text-align: center;
        padding: 15px 0 10px;
        font-weight: bold;
        margin-bottom: 0;
        color: #3A3C41;
        font-family: "Piedra", cursive;
        letter-spacing: 2px;
      }
      p {
        margin: 0;
        padding-bottom: 15px;
        text-align: center;
        color: #3A3C41;
      }
    }
    &:hover {
      box-shadow: 0 10px 10px rgba($color: #000000, $alpha: 0.3);
      .caption {
        bottom: 0px;
        z-index: 99;
      }
      .overlay {
        transform: translateX(0)!important;
      }
    }
  }
  button {
    margin: 30px auto;
    background: #3E4148;
    border-color: transparent;
    font-size: 18px;
    i {
      margin-left: 5px;
    }
    &:hover {
      transform: none;
      letter-spacing: 2px;
      i {
        padding-left: 6px;
        color: #FFB400;
      }
    }
  }
}
.contact {
  background: url('/imgs/contact.png');
  background-size: cover;
  .overlay {
    padding: 140px 0 200px;
    position: relative;
    h1 {
      text-align: center;
      text-transform: uppercase;
      font-family: "Piedra", cursive;
      position: relative;
      margin-bottom: 60px;
      &::after {
        content: "CONTACT";
        opacity: 0.1;
        font-size: 100px;
        width: 100%;
        text-align: center;
        display: block;
        position: absolute;
        top: -30px;
        left: 50%;
        transform: translateX(-50%);
        letter-spacing: 10px;
        @include md {
          font-size: 75px;
          top: -20px;
        }
        @include sm {
          font-size: 45px;
          top: -5px;
        }
      }
    }
    .info {
      @include sm {
        margin-bottom: 30px;
      }
      p {
        margin-top: 0;
        margin-bottom: 20px;
      }
      h4 {
        font-family: "Piedra", cursive;
        letter-spacing: 1.5px;
      }
      .way {
        margin-bottom: 20px;
        i {
          font-size: 20px;
          padding: 8px 11px;
          background: #FFB400;
          color: #fff;
          border-radius: 8px;
          box-shadow: 0 2px 6px rgba($color: #000000, $alpha: 0.2);
        }
        span {
          font-size: 18px;
        }
      }
      .icons {
        i {
          font-size: 34px;
          cursor: pointer;
          margin: 15px 10px 0 0;
          padding: 8px 10px;
          background: #fff;
          color: #FFB400;
          border-radius: 50%;
          &:first-of-type {
            padding: 8px 15px;
          }
          &:nth-of-type(2) {
            padding: 8px;
          }
          &:hover {
            background: #FFB400;
            color: #fff;
          }
        }
      }
    }
    .form {
      input, textarea {
        display: block;
        margin-bottom: 10px;
        background: rgba($color: #000000, $alpha: 0.2);
        width: 100%;
        border-radius: 20px;
        &::placeholder {
          color: #fff;
        }
        &:focus {
          border-color: #FFB400;
        }
      }
      button {
        background: #FFB400;
        border-color: #fff;
      }
    }
  }
}
</style>

<script>
/* eslint-disable */
export default {
  data () {
    return {
      name: '',
      email: '',
      message: '',
      portfolio: []
    }
  },
  methods: {
    open(link) {
      window.open(link, '_blank')
    },
    async sendMsg (e) {
       e.target.textContent = 'Sending...'
       const { name, email, message } = this

       if (name != '' && email != '' && message != '') {
         const DataToSend = { fullName: name, email, message }
         var res = await this.$axios.post('/api/contact', DataToSend)
         if (res.data._id) {
           this.name = ''
           this.email = ''
           this.message = ''
           alert('Your Message Sent Successfully')
         }
         e.target.textContent = 'Send Message'
       } else {
          e.target.textContent = 'Send Message'
          alert('Empty Fields!')
       }
    }
  },
  filters: {
     formatLangs (langs) {
        return langs.split(',').join(' | ')
     }
  },
  async created () {
    const portfolio = await this.$axios.$get('/api/portfolio')

    this.portfolio = portfolio

    const self = this
    setTimeout(() => {
      self.$store.commit('endLoading')
    }, 1000)
  }
}
</script>