
  <template>
   <v-app>
     <main class="main">
     <v-card
      color="rgba(0, 0, 0, 0.3)"
     dark
     >
         <v-card-actions>
   <v-text-field
            label="Ingrese una ciudad"
            ingrese una ciudad
          ></v-text-field>
      <v-btn text>
        Enviar
      </v-btn>
    </v-card-actions>
     </v-card>
  <v-card
    class="mx-auto main-card"
    max-width="400"
    color="rgba(0, 0, 0, 0.3)"
    dark
  >

    <v-list-item two-line>
      <v-list-item-content>
        <v-list-item-title class="text-h4">{{ weatherData.name }}</v-list-item-title>
        <v-list-item-title class="text h6">{{weatherData.sys.country}} , {{weatherData.weather[0].description}}</v-list-item-title>
      </v-list-item-content>
    </v-list-item>

    <v-card-text>
      <v-row align="center">
        <v-col
          class="text-h2"
          cols="12"
        >
          {{weatherData.main.temp}} F
        </v-col>
        <v-list-item>
      <v-list-item-title class="text-h7">Temp max: {{weatherData.main.temp_max}} F</v-list-item-title>
      <v-list-item-title class="text-h7">Temp min: {{weatherData.main.temp_min}} F</v-list-item-title>
    </v-list-item>
        <v-col cols="12">
          <v-img
            src="https://cdn.vuetifyjs.com/images/cards/sun.png"
            alt="Sunny image"
            width="92"
          ></v-img>
        </v-col>
      </v-row>
    </v-card-text>
 
    <v-list-item>
      <v-list-item-icon>
        <v-icon>mdi-send</v-icon>
      </v-list-item-icon>
      <v-list-item-title class="text-h5">Feels like: {{weatherData.main.feels_like}}</v-list-item-title>
    
    </v-list-item>
   
    <v-list-item>
      <v-list-item-icon>
        <v-icon>mdi-cloud-download</v-icon>
      </v-list-item-icon>
      <v-list-item-title class="text-h5">Humidity {{weatherData.main.humidity}}%</v-list-item-title>
    </v-list-item>

  </v-card>
   </main>
  </v-app>
  

</template>

<script>
export default {
  data() {
    return {
      weatherData: {},
    };
  },

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
  background: blue;
  place-items: center;
  min-height: 100vh;
  color: white;
  opacity: 0.5;
  
}

</style> 