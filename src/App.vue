
  <template>
  <v-app>
    <main
      class="main"
      :class="
        getBack(weatherData.weather ? weatherData.weather[0].description : null)
      "
    >
      <div v-if="classClouds" class="clouds">
        <div
          :style="`width: ${
            Math.random() * (700 - 100) + 100
          }px; position: absolute; top: ${
            Math.random() * (30 - 1) + 1
          }%; left: ${Math.random() * (100 - 1) + 1}%`"
          v-for="(cloud, i) in randomClouds()"
          :key="i"
        >
          <img style="width: 100%" src="./assets/cloud.png" alt="" />
        </div>
      </div>
      <div class="sun" v-if="description === 'clear sky'"></div>
      <v-container d-flex justify-center aling-center>
        <v-card
          width="100%"
          max-width="600"
          color="rgba(0, 0, 0, 0.3)"
          dark
          class="glass d-flex align-center py-2"
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
            class="mx-auto main-card glass pa-4 mt-2"
            max-width="600"
            width="100%"
            color="rgba(0, 0, 0, 0.3)"
            dark
            v-if="weatherData.name"
          >
            <h1>
              {{ weatherData.name }}
            </h1>

            <h3>{{ dateNow }}</h3>
            <h3>
              {{ weatherData.sys.country }}
              {{ weatherData.weather[0].description }}
            </h3>
        <v-row> 
            <v-col class="text-h2" cols-12>
              <h2>{{ parseInt(weatherData.main.temp) }} °C</h2>
            </v-col>

            <v-col cols="6" d-flex>
              <v-icon size="100" v-if="description === 'broken clouds'"
                >mdi-weather-partly-cloudy</v-icon
              >
              <v-icon size="100" v-else-if="description === 'few clouds'"
                >mdi-weather-partly-cloudy</v-icon
              >
              <v-icon size="100" v-else-if="description === 'scattered clouds'"
                >mdi-weather-partly-cloudy</v-icon
              >
              <v-icon size="100" v-else-if="description === 'thunderstorm'"
                >mdi-weather-weather-lightning</v-icon
              >
              <v-icon size="100" v-else-if="description === 'rain'"
                >mdi-weather-weather-pouring</v-icon
              >
              <v-icon size="100" v-else-if="description === 'shower rain'"
                >mdi-weather-weather-pouring</v-icon
              >
              <v-icon size="100" v-else-if="description === 'snow'"
                >mdi-weather-weather-snowy</v-icon
              >
              <v-icon size="100" v-else-if="description === 'mist'"
                >mdi-weather-weather-fog</v-icon
              >
              <v-icon size="100" v-else-if="description === 'clear sky'"
                >mdi-weather-sunny</v-icon
              >
              <v-icon size="100" v-else>mdi-weather-partly-cloudy</v-icon>
            </v-col>
            </v-row>

            <v-row>
              <v-col cols="6">
                <h6>Temperatura min:</h6>
                <p>{{ parseInt(weatherData.main.temp_min) }} °C</p>
              </v-col>
              <v-col cols="6">
                <h6>Temperatura max:</h6>
                <p>{{ parseInt(weatherData.main.temp_max) }} °C</p>
              </v-col>
              <v-col cols="12">
                <ul>
                  <li>
                    <v-icon>mdi-human-handsdown</v-icon> Feels like:
                    {{ weatherData.main.feels_like }}
                  </li>
                  <li>
                    <v-icon>mdi-human-handsdown</v-icon> Pressure:
                    {{ weatherData.main.pressure }}
                  </li>
                  <li>
                    <v-icon>mdi-cloud-download</v-icon> Humidity
                    {{ weatherData.main.humidity }}%
                  </li>
                </ul>
              </v-col>
            </v-row>
          </v-card>
        </v-expand-transition>
      </v-container>

      <div v-if="showError" class="error">
        <v-alert type="error">
          aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
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
      cityName: "Cordoba",
      showError: false,
      dateNow: null,
      loadingData: false,
      description: "",
      classClouds: false,
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
      /* console.log(weather) */
      if (weather == null) {
        return "";
      }
      let classes = "";

      if (weather == "broken clouds") {
        classes = "";
      } else if (weather == "few clouds") {
        classes = "";
      } else if (weather == "scattered clouds") {
        classes = "clouds";
      } else if (weather == "shower rain") {
        classes = "clouds";
      } else if (weather == "rain") {
        classes = "clouds";
      } else if (weather == "thunderstorm") {
        classes = "clouds";
      } else if (weather == "mist") {
        classes = "clouds";
      } else if (weather == "snow") {
        classes = "snow";
      } else classes = "";

      if (weather.includes("clouds")) {
        this.classClouds = true;
      }
      return classes;
    },

    randomClouds(number) {
      if (!this.description.includes("clouds")) {
        return 0;
      }
      number = 30;
      return Math.ceil(Math.random() * (number - 5) + 5);
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
}

.glass {
  background-color: rgb(255, 255, 255, 0.4);
  backdrop-filter: blur(10px);
}

.clouds {
  position: absolute;
  width: 100%;
  min-height: 100vh;
  overflow: hidden;
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

.cloudy {
  background: linear-gradient(160deg, grey 0%, #80d0c7 100%);
}

.snow {
  background: linear-gradient(160deg, lightblue 0%, #80d0c7 100%);
}

.error {
  position: absolute;
  bottom: 1rem;
  left: 50%;
  transform: translateX(-50%);
}
</style> 


