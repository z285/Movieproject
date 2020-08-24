<template>

  <div id="app" class="gradient">
    <link href="https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet" integrity="sha384-wvfXpqpZZVQGK6TAh5PVlGOfQNHSoD2xbE+QkPxCAFlNEevoEH3Sl0sibVcOQVnN" crossorigin="anonymous">
    <Header />
    <Movie />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Movie from './components/Movie.vue'

export default {
  name: 'App',
  components: {
    Header,
    Movie
  }
}


</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
 
  color: #2c3e50;
  margin-bottom: 20px;
}
#img {
  max-width: 30%;
  height: auto;
  height: 20px;
}

@import "./assets/flip.scss";
.gradient {
  background-image:
    linear-gradient(
      #101010, #ffffff
    );
}

</style>
