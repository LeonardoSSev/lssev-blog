<template>
  <div id="app">
   <ProfileCover />
    <div class="app-content">
      <router-view/>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import Component from 'vue-class-component';
import ProfileCover from './components/layout/profile-cover/profile-cover.vue';

@Component({
  components: {
    ProfileCover
  }
})
export default class App extends Vue {}
</script>

<style lang="scss">
#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  margin: 0;
  padding: 0;
  border: 0;
}
</style>
