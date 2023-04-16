<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title style="text-align: center">readyWriter</ion-title>
      </ion-toolbar>
      <div class="pagetitle">Audio Recordings</div>
    </ion-header>

    <ion-content :fullscreen="true">
      <div class="allSongs" v-for="(song, index) in songs" :key="index">
        <div class="title">{{ song.name }}</div>
        <audio
          ref="player"
          :src="src + song.audio"
          controls
          style="width: 100%; height: 3rem"
        />
      </div>
    </ion-content>
    <ion-fab slot="fixed" horizontal="end" vertical="bottom">
      <ion-fab-button size="large" color="primary" @click="isitrecording">
        <ion-icon :playing="playing" :icon="playing ? stop : mic"></ion-icon>
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
import { mic, stop } from "ionicons/icons";
import { ref } from "vue";
import {
  VoiceRecorder,
  VoiceRecorderPlugin,
  RecordingData,
  GenericResponse,
  CurrentRecordingStatus,
} from "capacitor-voice-recorder";
// import { useAVCircle } from "vue-audio-visual";
import ExploreContainer from "@/components/ExploreContainer.vue";

// const recicon = ref(mic);
const playing = ref(false);
const base64SoundString = localStorage.getItem("audio");
const base64Sound = base64SoundString ? JSON.parse(base64SoundString) : []; // from plugin
console.log(base64Sound);
const mimeType = "audio/webm;codecs=opus"; // from plugin
const audioRef = new Audio(`data:${mimeType};base64,${base64Sound}`);
const audioref = ref(`data:${mimeType};base64,${base64Sound}`);
const player = ref(null);
const canvas = ref(null);
const songs = ref(base64Sound);
const src = `data:audio/webm;codecs=opus;base64,`;
// audioRef.oncanplaythrough = () => audioRef.play();
// audioRef.load();

// useAVCircle(player, canvas, {
//   src: audioref,

//   barColor: "lime",
// });

async function isitrecording() {
  playing.value = !playing.value;
  const resp = await permission();
  if (playing.value) {
    if (resp) {
      // requestpermission();
      startrecord();
    } else {
      requestpermission();
      startrecord();
    }
  } else {
    stoprecord();
    window.location.reload();
  }
}
const recordValidate = async () => {
  try {
    const resp = await VoiceRecorder.canDeviceVoiceRecord();
    return resp.value;
  } catch (error) {
    console.log(error);
  }
};

/**
 * will prompt the user to give the required permission, after that
 * the function will print true / false based on the user response
 */

const requestpermission = async () => {
  try {
    const resp = await VoiceRecorder.requestAudioRecordingPermission();
    return resp.value;
  } catch (error) {
    console.log(error);
  }
};

/**
 * will print true / false based on the status of the recording permission.
 * the promise will reject with "COULD_NOT_QUERY_PERMISSION_STATUS"
 * if the current device cannot query the current status of the recording permission
 */
const permission = async () => {
  try {
    const resp = await VoiceRecorder.hasAudioRecordingPermission();
    // console.log(resp);
    return resp.value;
  } catch (error) {
    console.log(error);
  }
};
/**
 * In case of success the promise will resolve to { value: true }
 * in case of an error the promise will reject with one of the following messages:
 * "MISSING_PERMISSION", "ALREADY_RECORDING", "MICROPHONE_BEING_USED", "DEVICE_CANNOT_VOICE_RECORD", or "FAILED_TO_RECORD"
 */
async function startrecord() {
  await VoiceRecorder.startRecording()
    .then((result: GenericResponse) => console.log(result.value))
    .catch((error) => console.log(error));
}
// startrecord();
async function stoprecord() {
  await VoiceRecorder.stopRecording()
    .then((result: RecordingData) => {
      const oldaudiofilesString = localStorage.getItem("audio");
      const oldaudiofiles = oldaudiofilesString
        ? JSON.parse(oldaudiofilesString)
        : [];
      console.log(oldaudiofiles);
      const d = new Date();
      let ms = d.getMilliseconds();
      const newaudiofiles = {
        name: ms,
        audio: result.value.recordDataBase64,
      };
      oldaudiofiles.push(newaudiofiles);
      localStorage.setItem("audio", JSON.stringify(oldaudiofiles));
      console.log(result.value);
    })
    .catch((error) => console.log(error));
}
// stoprecord();

async function pauserecord() {
  await VoiceRecorder.pauseRecording()
    .then((result: GenericResponse) => console.log(result.value))
    .catch((error) => console.log(error));
}
/**
 * will resume a paused recording. note that if the recording has not started yet the promise
 * will reject with `RECORDING_HAS_NOT_STARTED`. in case of success the promise will resolve to `{ value: true }` if the resume
 * was successful or `{ value: false }` if the recording is already running.
 * if the current mobile os does not support this method the promise will reject with `NOT_SUPPORTED_OS_VERSION`
 */
async function resumerecord() {
  await VoiceRecorder.resumeRecording()
    .then((result: GenericResponse) => console.log(result.value))
    .catch((error) => console.log(error));
}
/**
 * Will return the current status of the plugin.
 * in this example one of these possible values will be printed: "NONE" / "RECORDING" / "PAUSED"
 */
async function recordstatus() {
  await VoiceRecorder.getCurrentStatus()
    .then((result: CurrentRecordingStatus) => console.log(result.status))
    .catch((error) => console.log(error));
}
</script>

<style scoped>
.pagetitle {
  background: var(--ion-color-primary);
  padding: 0.5rem;
}
.allSongs {
  background: rgba(255, 255, 255, 0.329);
  padding: 1rem;
}
.title {
  font-size: 1.3rem;
  padding-bottom: 0.5rem;
}
</style>
