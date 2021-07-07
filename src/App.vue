<template>
  <v-app>
   <main class="main">
    <v-card max-width="800"> 
      <h1> {{weatherData.name}} </h1> 
    </v-card>
   </main>
  </v-app>
</template>

<script>

export default {
  name: 'App',
  data: () => ({
    weatherData: {}
  }),

  mounted() {
    fetch('https://api.openweathermap.org/data/2.5/weather?q=Cordoba&appid=8de24b65852e6b354d3e60e84a5a209b')
      .then((res) => res.json())
      .then(data => {
      this.weatherData = data;
      console.log("Esto es weather info", this.weatherData)
      })
  }
};
</script>

<style scoped>
.main { display: grid;
        place-items: center;
        background: grey;
        min-height: 100vh;
        color: white;
     }

</style>