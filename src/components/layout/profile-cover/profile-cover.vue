<template>
  <div :class="`profile-cover ${profileCoverClass}`" @click="toggleCover()">
    <div class="profile-content">
      <ProfileLoader v-if="isLoading" />
      <div class="profile-img" v-if="!isLoading">
        <img :src="profile.avatar_url" :alt="profile.login" />
      </div>
      <ProfileDivider />
      <div class="profile-details" v-if="!isLoading">
        <p class="name">
          {{ profile.name }}
          <span class="login">
            || <span class="login-text">{{ profile.login }}</span>
          </span>
        </p>
        <p class="description">{{ profile.bio }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import Component from 'vue-class-component';
import ProfileLoader from '@/components/shared/profile-loader/profile-loader.vue';
import ProfileDivider from '@/components/shared/profile-divider/profile-divider.vue';

@Component({
  components: {
    ProfileLoader,
    ProfileDivider
  }
})
export default class ProfileCover extends Vue {
  GITHUB_USER = 'leonardossev';
  isCoverFull = true;
  isLoading = true;

  mounted() {
    this.startCover();
  }

  startCover() {
    if (!this.personalInfoExist()) {
      this.getPersonalInfo();
      return;
    }
    this.setLoading(false);
  }

  personalInfoExist() {
    return localStorage.getItem('personalInfo');
  }

  getPersonalInfo() {
    this.setLoading(true);
    setTimeout(() => {
      fetch(`https://api.github.com/users/${this.GITHUB_USER}`)
        .then(response => response.json())
        .then(data => {
          this.savePersonalInfo(JSON.stringify(data));
          this.setLoading(false);
        });
    }, 3000);
  }

  setLoading(isLoading) {
    this.isLoading = isLoading;
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

  get profile() {
    return JSON.parse(localStorage.getItem('personalInfo'));
  }
}
</script>

<style lang="scss">
/* Propriedades comuns */
.profile-cover {
  background: #000 no-repeat center center fixed;
  background-size: cover;
  width: 100%;
  color: #fff;
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
      display: flex;
      justify-content: center;
      align-content: center;

      img {
        border: 2px solid #fff;
        border-radius: 50%;
        padding: 0.5%;
        max-width: 50%;
        height: auto;
      }
    }

    .profile-details {
      text-align: center;
      margin: 0 1% 0 1%;

      .name {
        font-weight: 700;
        font-size: 22px;
      }

      .login {
        font-weight: 300;
        color: #c2c2c2;

        .login-text {
          font-style: italic;
        }
      }
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
          animation: shrink 0.5s ease-in;
          width: 30%;
          margin: auto;
        }
      }

      .profile-details,
      .profile-divider {
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
</style>
