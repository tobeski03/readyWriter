<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title style="text-align: center">readyWriter</ion-title>
      </ion-toolbar>
      <div class="pagetitle">Notes</div>
    </ion-header>
    <ion-content class="ion-padding" :fullscreen="true">
      <!-- <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">readyWriter</ion-title>
        </ion-toolbar>
      </ion-header> -->
      <div
        class="jotList ion-padding"
        v-for="(note, index) in notes"
        :key="index"
      >
        <div class="titlecontainer" style="border-bottom: 1px solid white">
          <div class="title" style="">
            {{ note.title }}
          </div>

          <ion-icon
            :icon="ellipsisVertical"
            :id="`open-action-sheet[${index}]`"
          ></ion-icon>
          <ion-action-sheet
            :trigger="`open-action-sheet[${index}]`"
            header="Actions"
            :buttons="actionSheetButtons"
          ></ion-action-sheet>
        </div>
        {{ note.message }}
      </div>

      <!-- <ExploreContainer name="Tab 1 page" /> -->
    </ion-content>
    <ion-fab slot="fixed" horizontal="end" vertical="bottom">
      <ion-fab-button color="primary">
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
  IonFab,
  IonFabButton,
  IonIcon,
  IonActionSheet,
  IonButton,
} from "@ionic/vue";
import { ref, defineComponent } from "vue";
import { add, ellipsisVertical } from "ionicons/icons";
import ExploreContainer from "@/components/ExploreContainer.vue";

const notes = ref([
  {
    title: "Sunday Sermon",
    message: `Lorem ipsum dolor sit, amet consectetur adipisicing elit. Alias
        consectetur, omnis aspernatur beatae, praesentium possimus illum
        veritatis nihil officia ut iusto numquam delectus! Voluptates officiis,
        a nesciunt obcaecati odio quos.`,
  },
  {
    title: "Friday Sermon",
    message: `Lorem ipsum dolor sit, amet consectetur adipisicing elit. Alias
        consectetur, ! Voluptates officiis,
        a nesciunt obcaecati odio quos.`,
  },
  {
    title: "Wednesday Message",
    message: `Lorem ipsum dolor sit, amet consectetur adipisicing elit. Alias
        consectetur, omnis aspernatur beatae, praesentium possimus illum
        veritatis nihil officia ut iusto numquam delectus! Voluptates officiis,
        a nesciunt  odio quos.`,
  },
  {
    title: "Sunday Sermon",
    message: `Lorem ipsum dolor sit, amet consectetur adipisicing elit. Alias
        consectetur, omnis aspernatur beatae, praesentium possimus illum
        veritatis nihil officia ut iusto numquam delectus! Voluptates officiis,
        a nesciunt obcaecati odio quos.`,
  },
  {
    title: "Friday Sermon",
    message: `Lorem ipsum dolor sit, amet consectetur adipisicing elit. Alias
        consectetur, ! Voluptates officiis,
        a nesciunt obcaecati odio quos.`,
  },
  {
    title: "Wednesday Message",
    message: `Lorem ipsum dolor sit, amet consectetur adipisicing elit. Alias
        consectetur, omnis aspernatur beatae, praesentium possimus illum
        veritatis nihil officia ut iusto numquam delectus! Voluptates officiis,
        a nesciunt  odio quos.`,
  },
]);
const actionSheetButtons = ref([
  {
    text: "Delete",
    role: "destructive",
    data: {
      action: "delete",
    },
  },
  {
    text: "Share",
    data: {
      action: "share",
    },
  },
  {
    text: "Cancel",
    role: "cancel",
    data: {
      action: "cancel",
    },
  },
]);
</script>
<style scoped>
.jotList {
  background: var(--ion-color-tertiary-shade);
  border-radius: 1.5rem;
  margin: 1rem 0;
}
.pagetitle {
  background: var(--ion-color-primary);
  padding: 0.5rem;
}
.titlecontainer {
  display: flex !important;
  justify-content: space-between;
  margin-bottom: 0.3rem;
}
.title {
  font-size: 1.3rem;

  padding-bottom: 0.2rem;
}
</style>
