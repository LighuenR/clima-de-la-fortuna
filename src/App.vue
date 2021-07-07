<template>
  <v-app>
    <main class="main">
      <v-card>
        <input type="text" placeholder="Enter a city"> <button>Enviar</button>
      </v-card>
      <v-card max-width="800">
        <div>
          <h3>{{ weatherData.name }}</h3>
          <p>{{ weatherData.weather[0].description }}</p>
        </div>

        <div>
          <h1>{{weatherData.main.temp}}</h1>
          <p>Min: {{weatherData.main.temp_min}}</p>
          <p>Max: {{weatherData.main.temp_max}}</p>
           <p>Feels like: {{weatherData.main.feels_like}}</p>
          
        </div>
      </v-card>
    </main>
  </v-app>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    weatherData: {},
  }),

  mounted() {
    fetch(
      "https://api.openweathermap.org/data/2.5/weather?q=Cordoba&appid=8de24b65852e6b354d3e60e84a5a209b"
    )
      .then((res) => res.json())
      .then((data) => {
        this.weatherData = data;
        console.log("Esto es weather info", this.weatherData);
      });
  },
};
</script>

<style scoped>
.main {
  display: grid;
  place-items: center;
  background: grey;
  min-height: 100vh;
  color: white;
}
</style>