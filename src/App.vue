
  <template>
   <v-app>
     <v-card>
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
    class="mx-auto"
    max-width="400"
  >

    <v-list-item two-line>
      <v-list-item-content>
        <v-list-item-title class="text-h5">{{ weatherData.name }}</v-list-item-title>
        <v-list-item-subtitle>{{weatherData.sys.country}} , {{weatherData.weather[0].description}}</v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>

    <v-card-text>
      <v-row align="center">
        <v-col
          class="text-h2"
          cols="5"
        >
          {{weatherData.main.temp}}&deg;C
        </v-col>
        <v-col cols="6">
          <v-img
            src="https://cdn.vuetifyjs.com/images/cards/sun.png"
            alt="Sunny image"
            width="92"
          ></v-img>
        </v-col>
      </v-row>
    </v-card-text>
 <v-list-item>
        <v-list-item-subtitle>Min: {{weatherData.main.temp_min}}</v-list-item-subtitle>
      <v-list-item-subtitle>Min: {{weatherData.main.temp_max}}</v-list-item-subtitle>
    </v-list-item>
    <v-list-item>
      <!-- <v-list-item-icon>
        <v-icon>mdi-send</v-icon>
      </v-list-item-icon> -->
      <v-list-item-subtitle>Feels like: {{weatherData.main.feels_like}}</v-list-item-subtitle>
    
    </v-list-item>
   
    <v-list-item>
      <v-list-item-icon>
        <v-icon>mdi-cloud-download</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>{{weatherData.main.humidity}}</v-list-item-subtitle>
    </v-list-item>


    <v-list class="transparent">
      <v-list-item
        v-for="item in forecast"
        :key="item.day"
      >
        <v-list-item-title>{{ item.day }}</v-list-item-title>

        <v-list-item-icon>
          <v-icon>{{ item.icon }}</v-icon>
        </v-list-item-icon>

        <v-list-item-subtitle class="text-right">
          {{ item.temp }}
        </v-list-item-subtitle>
      </v-list-item>
    </v-list>


  </v-card>
  </v-app>
  

</template>

<script>
export default {
    data () {
      return {
       
     weatherData: {},
       
       
      }
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
/* .main {
  display: grid;
  place-items: center;
  background: grey;
  min-height: 100vh;
  color: white; 
}*/
</style> 