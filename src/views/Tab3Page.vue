<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title style="text-align: center">readyWriter</ion-title>
      </ion-toolbar>
      <div class="pagetitle">To-Do List</div>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 3</ion-title>
        </ion-toolbar>
      </ion-header>

      <ExploreContainer name="Tab 3 page" />
    </ion-content>
    <ion-fab slot="fixed" horizontal="end" vertical="bottom">
      <ion-fab-button size="large" color="primary">
        <ion-icon :icon="add"></ion-icon>
      </ion-fab-button>
    </ion-fab>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
} from "@ionic/vue";
import { add } from "ionicons/icons";
import ExploreContainer from "@/components/ExploreContainer.vue";
</script>
