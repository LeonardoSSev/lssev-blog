<template>
  <div class="home">
    <ProfileCover/>
  </div>
</template>
<script>

import Vue from 'vue';
import Component from 'vue-class-component';
import ProfileCover from '@/components/layout/profile-cover.vue';

@Component({
  components: {
    'ProfileCover': ProfileCover
  }
})
export default class Home extends Vue {

}
</script>
