<template>
  <div>
    <HeroImage />
    <Movies/>
  </div>
</template>

<script>
import Movies from '../components/Movies.vue';
export default {
  components: {Movies },
  name: "IndexPage",
};
</script>
