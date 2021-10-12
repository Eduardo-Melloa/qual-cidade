<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Qual cidade estou?</ion-title>
      </ion-toolbar>
    </ion-header>
  
  
    <ion-content>    
      <div class="container">
        <h1>Coordenadas</h1>
        <h2>{{ latitude }}</h2>
        <h3>{{ longitude }}</h3>
        <ion-button @click='buscaCidade()'>Qual cidade?</ion-button>
        <h3>{{ cidade }}</h3>
      </div>

      
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonButton } from '@ionic/vue';
import { defineComponent } from 'vue';
import { Geolocation } from '@capacitor/geolocation';
import { Http } from '@capacitor-community/http';

export default defineComponent({
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonButton,
    IonToolbar
  },
  data(){
    return {
      latitude:0,
      longitude:0,
      cidade:''
    }
  },
  ionViewWillEnter() {
    this.printCurrentPosition();
  },
  methods: {
    printCurrentPosition: async function(){
      const coordinates = await Geolocation.getCurrentPosition();
      console.log('Curent position:', coordinates);
      this.latitude = coordinates.coords.latitude;
      this.longitude = coordinates.coords.longitude;
    },
    buscaCidade: async function(){
      const ACCESS_KEY = 'bf9a3b189486a642edc9e6d3cb9259ac';
      const options = {
        url: `http://api.positionstack.com/v1/reverse?access_key=${ACCESS_KEY}&query=${this.latitude},${this.longitude}&limit=1`
      };
      const response = await Http.get(options);
      console.log(response);

      this.cidade = response.data.data[0].locality + ', '+ response.data.data[0].region_code;
    }
  },
  
});
</script>

<style scoped>
.container{
  text-align: center;
}
</style>