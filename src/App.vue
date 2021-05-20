<template>
  <Navbar />
  <router-view class="container"/>
</template>

<script>
import { defineComponent } from 'vue';
import Navbar from './components/Navbar.vue';

export default defineComponent({
  components: {
    Navbar,
  },
  setup() {

  },
});
</script>

<style lang="scss">

</style>
