<template>
  <div id="app">
    <div :class="`profile-cover ${profileCoverClass}`">
      <div class="profile-content">
        <div class="profile-img">
          <img :src="avatar" :alt="login" @click="toggleCover()">
        </div>
        <div class="profile-details">
          <p class="name">{{ name }}</p>
          <p class="description">{{ bio }}</p>
        </div>
      </div>
    </div>
    <div class="app-content">
      <router-view/>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import Component from 'vue-class-component';

@Component
export default class App extends Vue {
  GITHUB_USER = 'leonardossev';

  name = 'Leonardo Santos';
  description = 'Transformo bom café e ódio em código saudável.';
  isCoverFull = true;

  mounted() {
    this.startApp();
  }

  startApp() {
    if (!this.personalInfoExist()) {
      this.getPersonalInfo();
    }
  }

  personalInfoExist() {
    return localStorage.getItem('personalInfo');
  }

  getPersonalInfo() {
    fetch(`https://api.github.com/users/${this.GITHUB_USER}`)
      .then(response => response.json())
      .then(data => this.savePersonalInfo(JSON.stringify(data)));
  }

  savePersonalInfo(personalInfo) {
    localStorage.setItem('personalInfo', personalInfo);
  }

  toggleCover() {
    this.isCoverFull = !this.isCoverFull;
  }

  get profileCoverClass() {
    return this.isCoverFull === true ? 'full' : 'half';
  }

  get avatar() {
    const personalInfo = JSON.parse(localStorage.getItem('personalInfo'));

    return personalInfo.avatar_url;
  }

  get login() {
    const personalInfo = JSON.parse(localStorage.getItem('personalInfo'));

    return personalInfo.login;
  }

  get bio() {
    const personalInfo = JSON.parse(localStorage.getItem('personalInfo'));

    return personalInfo.bio;
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  margin: 0;
  padding: 0;
  border: 0;

  /* Propriedades comuns */
  .profile-cover {
    background: #000 no-repeat center center fixed;
    background-size: cover;
    width: 100%;
    color: #FFF;
    position: fixed;
    top: 0;
    left: 0;
    display: flex;
    justify-content: center;
    align-content: center;
    transition: transform 2s;

    &.full {
      max-width: 100%;
      transition: all 0.8s ease;
    }

    .profile-content {
      display: flex;
      align-items: center;
      justify-content: center;
      align-content: center;
      flex-direction: column;

      .profile-img {
        padding: 0 10% 0 10%;
        align-content: center;

        img {
          border: 2px solid #FFF;
          border-radius: 50%;
          padding: 0.5%;
          cursor: pointer;
          max-width: 100%;
          height: auto;
        }
      }

      .profile-details {
        text-align: center;
      }
    }
  }

  /* Dispositivos médios/grandes */
  @media only screen and (min-width: 730px) {
    .profile-cover {
      &.full {
        height: 100%;
      }

      &.half {
        max-width: 40%;
        height: 100%;
        transition: max-width 0.8s ease;
      }
    }
  }

  /* Dispositivos pequenos */
  @media only screen and (max-width: 729px) {
    .profile-cover {
      &.full {
        height: 100%;
        max-height: 100%;

        .profile-content {
          .profile-img {
            display: flex;

            img {
              animation: grow 0.8s ease-out;
              margin: auto;
              width: 70%;
            }
          } 

          .profile-details {
            animation: appear 0.8s ease-in;
          }
        }
      }

      &.half {
        height: 20%;
        transition: all 0.8s ease;

        .profile-content > .profile-img {
          display: flex;

          img {
            animation: shrink 0.8s ease-in;
            width: 30%;
            margin: auto;
          }
        }

        .profile-details {
          display: none;
        }
      }

      /* Efeito para aparecer aos poucos */
      @keyframes appear {
        0% {
          opacity: 0;
        }
        100% {
          opacity: 1;
        }
      }

      /* Efeito para aumentar aos poucos */
      @keyframes grow {
        0% {
          width: 30%;
        }

        100% { 
          width: 70%;
        }
      }

      /* Efeito para diminuir aos poucos */
      @keyframes shrink {
        0% {
          width: 70%;
        }

        100% { 
          width: 30%;
        }
      }
    }
  }
}
</style>
