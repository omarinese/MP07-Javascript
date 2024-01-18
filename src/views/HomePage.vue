<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Time Fighter</ion-title>
        <ion-buttons slot="primary">
          <ion-button color="primary" fill="solid" @click="presentaAlerta">
            <ion-icon :icon="iconaInfo"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header class="ion-no-border ion-padding-top ion-padding-horizontal">
        <ion-grid>
          <ion-row>
            <ion-col>
              <div class="ion-text-start">
                La teva puntuació: {{ puntuacio }}
              </div>
            </ion-col>
            <ion-col>
              <div class="ion-text-end">
                Temps restant: {{ tempsRestant }}
              </div>
            </ion-col>
          </ion-row>
        </ion-grid>
      </ion-header>

      <div id="container" @click="animateContainer">
        <ion-button color="primary" @click="tocar" class="animated-button">
          Toca'm
        </ion-button>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonButton,
  IonButtons,
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  IonIcon,
  IonGrid,
  IonCol,
  alertController,
  toastController,
} from '@ionic/vue';
import { ref, watch } from 'vue';
import { informationCircleOutline } from 'ionicons/icons';
const iconaInfo = informationCircleOutline;
import * as THREE from 'three';

let puntuacio = ref(0);
let tempsRestant = ref(5);
let temporitzador: any = null;
let temporitzadorIniciat = false;

const presentaAlerta = async () => {
  const alerta = await alertController.create({
    header: 'Time Fighter',
    subHeader: 'Creat per Oriol Mariné Sevilla',
    message: 'MP07 JAVASCRIPT',
    buttons: ['Tanca'],
  });

  await alerta.present();
};

const tocar = async () => {
  console.log('tocar');
  puntuacio.value += 1;
  const toast = await toastController.create({
    message: 'Has tocat el botó!',
    duration: 2000,
  });
  await toast.present();

  if (!temporitzadorIniciat) {
    iniciaTemporitzador();
    temporitzadorIniciat = true;
  }
};

const mostraAlertaPuntuacio = async () => {
  const alertaPuntuacio = await alertController.create({
    header: 'Fi del joc',
    subHeader: `La teva puntuació final és ${puntuacio.value}`,
    buttons: ['Tanca'],
  });

  await alertaPuntuacio.present();

  reiniciaJoc();
};

const reiniciaJoc = () => {
  puntuacio.value = 0;
  tempsRestant.value = 5;
  temporitzadorIniciat = false;
  clearInterval(temporitzador);
};

const iniciaTemporitzador = () => {
  temporitzador = setInterval(() => {
    if (tempsRestant.value > 0) {
      tempsRestant.value -= 1;
    } else {
      clearInterval(temporitzador);
      mostraAlertaPuntuacio();
    }
  }, 1000);
};

watch(() => tempsRestant, () => {
  if (tempsRestant.value === 0) {
    clearInterval(temporitzador);
    mostraAlertaPuntuacio();
  }
});
</script>

<style scoped>
#container {
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  color: #8c8c8c;
  margin: 0;
}

#container a {
  text-decoration: none;
}

.animated-container {
  animation: shake 0.5s;
}

.animated-button {
  animation: bounce 0.5s;
}

@keyframes shake {
  0% { transform: translateX(0); }
  20% { transform: translateX(-5px); }
  40% { transform: translateX(5px); }
  60% { transform: translateX(-5px); }
  80% { transform: translateX(5px); }
  100% { transform: translateX(0); }
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
  40% { transform: translateY(-10px); }
  60% { transform: translateY(-5px); }
}
</style>