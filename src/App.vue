
  <template>
  <v-app>
    <main class="main" :class="getBack(weatherData.weather ? weatherData.weather[0].description : null)">
      <v-card color="rgba(0, 0, 0, 0.3)" dark>
        <v-card-actions>
          <!--   <v-form
          class="d-inline-flex"
            @submit="
              getCurrentWeather();
              
            "
            id="sendCity"
          > -->
          <v-text-field
           @keyup.enter="getCurrentWeather();
              expand = !expand;"
            label="Ingrese una ciudad"
            ingrese
            una
            ciudad
            v-model="cityName"
          >
          </v-text-field>
          <v-btn
            @click="
              getCurrentWeather();
              expand = !expand;
            "
            form="sendCity"
            text
            :loading="loadingData"
          >
            Enviar
          </v-btn>
        </v-card-actions>
      </v-card>

      <v-expand-transition>
        <v-card
          class="mx-auto main-card"
          max-width="400"
          color="rgba(0, 0, 0, 0.3)"
          dark
          v-if="showCard"
        >
          <v-list-item two-line>
            <v-list-item-content>
              <v-list-item-title class="text-h4">{{
                weatherData.name
              }}</v-list-item-title>
              
              <v-list-item-title class="text h5"
                >{{ dateNow }}</v-list-item-title
              >

              <v-list-item-title class="text h5"
                >{{ weatherData.sys.country }},
                {{ weatherData.weather[0].description }}</v-list-item-title
              >
            </v-list-item-content>
          </v-list-item>

          <v-card-text>
            <v-row align="center">
              <v-col class="text-h2" cols="6">
                {{ parseInt(weatherData.main.temp) }} °C
              </v-col>
              <v-col cols="6">
                <v-icon size="110">mdi-weather-sunny</v-icon>
              </v-col>
              <v-list-item>
                <v-list-item-title class="text-h7"
                  >Min:
                  {{
                    parseInt(weatherData.main.temp_min)
                  }}
                  °C</v-list-item-title
                >
                <v-list-item-title class="text-h7"
                  >Max:
                  {{
                    parseInt(weatherData.main.temp_max)
                  }}
                  °C</v-list-item-title
                >
              </v-list-item>
            </v-row>
          </v-card-text>

          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-human-handsdown</v-icon>
            </v-list-item-icon>
            <v-list-item-title class="text-h7"
              >Feels like: {{ weatherData.main.feels_like }}</v-list-item-title
            >
          </v-list-item>

          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-human-handsdown</v-icon>
            </v-list-item-icon>
            <v-list-item-title class="text-h7"
              >Pressure: {{ weatherData.main.pressure }}</v-list-item-title
            >
          </v-list-item>

          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-cloud-download</v-icon>
            </v-list-item-icon>
            <v-list-item-title class="text-h7"
              >Humidity {{ weatherData.main.humidity }}%</v-list-item-title
            >
          </v-list-item>
        </v-card>
      </v-expand-transition>
    </main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      weatherData: {},
      cityName: "Cordoba",
      showCard: false,
      dateNow: null,
      loadingData: false,
      
    }
  },
  mounted() {
    var options = { year: 'numeric', month: 'numeric', day: 'numeric' };
    this.dateNow = new Date().toLocaleDateString(options);
  },

  methods: {
    getCurrentWeather() {
      this.loadingData = true
      fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.cityName}&appid=8de24b65852e6b354d3e60e84a5a209b&units=metric`
      )
        .then((res) => res.json())
        .then((data) => {
          this.weatherData = data;
          this.showCard = true;
          this.loadingData = false;
        });
    },
   
   getBack(weather) {
     console.log(weather)
      if(weather == null){
      return ""}
      else if(weather == "broken clouds") {
      return "sunny"}
      else if(weather == "few clouds") {return ""}
      else if(weather == "scattered clouds") {return "clouds"}
      else if(weather == "broken clouds") {return "clouds"}
      else if(weather == "shower rain") {return "clouds"}
      else if(weather == "rain") {return "clouds"}
      else if(weather == "thunderstorm") {return "clouds"}
      else if(weather == "mist") {return "clouds"}
      else if(weather == "snow") {return "snow"}
      else return ""
      
    },

    
  },
};
</script>

<style scoped>
.main {
  display: grid;
  background: linear-gradient(160deg, blue 0%, #80d0c7 100%);
  place-items: center;
  min-height: 100vh;
  color: white;
  opacity: 0.5;
}

.sunny {
  background: linear-gradient(160deg, red 0%, #80d0c7 100%); 
}

.clouds {
  background: linear-gradient(160deg, grey 0%, #80d0c7 100%); 
}

.snow {
  background: linear-gradient(160deg, lightblue 0%, #80d0c7 100%); 
 }
</style> 


