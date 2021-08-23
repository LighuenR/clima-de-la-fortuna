
  <template>
  <v-app>
    <main
      class="main"
      :class="
        getBack(weatherData.weather ? weatherData.weather[0].description : null)
      "
    >
      <!-- El sol de fondo -->
      <div v-if="classSun" class="sun"></div>
      <!-- Las nubes de fondo -->
      <div v-if="classClouds" class="clouds">
        
      </div>

      <v-container d-flex justify-center align-center>
        <v-card
          width="100%"
          max-width="400"
          color="rgba(0, 0, 0, 0.3)"
          dark
          class="glass d-flex align-center pa-2"
        >
          <v-text-field
            @keydown.enter="getCurrentWeather()"
            label="Ingrese una ciudad"
            v-model="cityName"
            outlined
            hide-details
          >
          </v-text-field>
          <v-btn
            @click="getCurrentWeather()"
            form="sendCity"
            text
            :loading="loadingData"
          >
            Enviar
          </v-btn>
        </v-card>
      </v-container>
      <v-container>
        <v-expand-transition>
          <v-card
            class="mx-auto main-card glass pa-4 mt-2 d-grid"
            max-width="400"
            width="100%"
            color="rgba(0, 0, 0, 0.3)"
            dark
            v-if="weatherData.name"
          >
            <v-row class="align-center">
              <v-col cols="12">
                <h1>
                  {{ weatherData.name }}
                </h1>

                <h3>{{ dateNow }}</h3>
                <h3>
                  {{ weatherData.sys.country }}
                  {{ weatherData.weather[0].description }}
                </h3>
              </v-col>
            </v-row>

            <v-row>
              <v-col class="text-h2" cols="12">
                <h2>{{ parseInt(weatherData.main.temp) }}°C</h2>
              </v-col>
              <v-col cols="6" class="pb-0">
                <h4>Temperature min:</h4>
                <p>{{ parseInt(weatherData.main.temp_min) }} °C</p>
              </v-col>
              <v-col cols="6" class="pb-0">
                <h4>Temperature max:</h4>
                <p>{{ parseInt(weatherData.main.temp_max) }} °C</p>
              </v-col>
            </v-row>

            <v-row>
              <v-col cols="12" class="pa-0">
                <v-icon size="100" v-if="description === 'broken clouds'"
                  >mdi-weather-partly-cloudy</v-icon
                >
                <v-icon size="100" v-else-if="description === 'few clouds'"
                  >mdi-weather-partly-cloudy</v-icon
                >
                <v-icon
                  size="110"
                  v-else-if="description === 'scattered clouds'"
                  >mdi-weather-partly-cloudy</v-icon
                >
                <v-icon size="110" v-else-if="description === 'thunderstorm'"
                  >mdi-weather-weather-lightning</v-icon
                >
                <v-icon size="110" v-else-if="description === 'rain'"
                  >mdi-weather-weather-pouring</v-icon
                >
                <v-icon size="110" v-else-if="description === 'shower rain'"
                  >mdi-weather-weather-pouring</v-icon
                >
                <v-icon size="110" v-else-if="description === 'snow'"
                  >mdi-weather-weather-snowy</v-icon
                >
                <v-icon size="110" v-else-if="description === 'mist'"
                  >mdi-weather-weather-fog</v-icon
                >
                <v-icon size="110" v-else-if="description === 'clear sky'"
                  >mdi-weather-sunny</v-icon
                >
                <v-icon size="110" v-else>mdi-weather-partly-cloudy</v-icon>
              </v-col>
           <v-container>
              <v-col cols="12">
                <ul class="pa-0">
                  <li>
                    Feels like: {{ parseInt(weatherData.main.feels_like) }} °C |
                  </li>
                  <li>Pressure: {{ weatherData.main.pressure }} | </li>
                  <li>Humidity: {{ weatherData.main.humidity }}%</li>
                </ul>
              </v-col>
              </v-container>
            </v-row>
          </v-card>
        </v-expand-transition>
      </v-container>

      <!-- Error cuando la ciudad ingresada no existe -->
      <div v-if="showError" class="error">
        <v-alert type="error">
          No se ha encontrado la ciudad ingresada
        </v-alert>
      </div>
    </main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      weatherData: {},
      cityName: "",
      showError: false,
      dateNow: null,
      loadingData: false,
      description: "",
      classClouds: false,
      classSun: false,
    
    };
  },

  mounted() {
    var options = { year: "numeric", month: "numeric", day: "numeric" };
    this.dateNow = new Date().toLocaleDateString(options);
  },

  methods: {
    async getCurrentWeather() {
      this.loadingData = true;
      fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.cityName}&appid=8de24b65852e6b354d3e60e84a5a209b&units=metric`
      )
        .then((res) => {
          if (res.ok) {
            return res.json();
          } else {
            this.loadingData = false;
            this.showError = true;
            setTimeout(() => {
              this.showError = false;
            }, 3000);
            throw new Error("Error fetching data");
          }
        })
        .then((data) => {
          this.weatherData = data;
          this.description = data.weather[0].description;
          this.loadingData = false;
          this.expand = !this.expand;
          this.loadingData = false;

        })
        .catch((err) => console.log(err));
    },

    getBack(weather) { 
      console.log(this.weatherData)
      if (weather == null) {
        return "";
      }
      let classes = "";
      if (weather.includes("clouds")) {
        this.classClouds = true;
        
} 
      if (weather === "clear sky") {
        this.classSun = true;
      }
      else if(this.weatherData.weather[0].icon[this.weatherData.weather[0].icon.length-1] === "n") {
        this.classSun = false;
        return " night"
      }

      return classes;
    },
  
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Lato&display=swap');

.main {
  display: grid;
  background: linear-gradient(160deg, blue 0%, #80d0c7 100%);
  place-items: center;
  min-height: 100vh;
  color: white;
  text-align: center;
  font-family: "Lato";
}

.night {
  background: linear-gradient(160deg, black 0%, rgba(0,0,0, 0.3) 100%);
}

.glass {
  background-color: rgb(255, 255, 255, 0.4);
  backdrop-filter: blur(10px);
}

li {
  list-style: none;
  display: inline;
  padding: 0;
}

.clouds {
  position: absolute;
  width: 100%;
  min-height: 100vh;
  overflow: hidden;
  background: url("./assets/cloud.png");
  background-size: 600px;
  background-position: 80% 20%;
  background-repeat: no-repeat;


}
.sun {
  position: absolute;
  width: 100%;
  min-height: 100vh;
  overflow: hidden;
  background: url("./assets/sun.png");
  background-size: 200px;
  background-repeat: no-repeat;
  background-position: 80% 20%;
  opacity: 0.7;
}



/* 
.snow {
  background: linear-gradient(160deg, lightblue 0%, #80d0c7 100%);
} */

.error {
  position: absolute;
  bottom: 1rem;
  left: 50%;
  transform: translateX(-50%);
  width: 70%;
}
</style> 


