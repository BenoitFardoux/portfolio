<script setup lang="ts">


import {Ref, onMounted, ref} from "vue";
import {API, projet} from "../Utils";
import axios, { AxiosError, AxiosResponse } from "axios";

const isLoading = ref(true);
const hasError = ref(false);
const toDisplay : Ref<any> = ref({});

onMounted(async () => {
  axios.get(API + 'projects/', {
    headers: {
      'Content-Type': 'application/json',
    },
    method: 'GET'
  }).then( (res: AxiosResponse) => {
    isLoading.value = false;
      toDisplay.value = res.data.data
      console.log(toDisplay.value[0].title.length)

  },(error : AxiosError) => console.log(error))
})
// console.log(experience)
</script>

<template>
  <div class="popup">
    <h2>
      Mon dernier projet
    </h2>
    <span v-if="isLoading">
      <img src="../../assets/Loading.svg" alt="loading">
    </span>
    <span v-if="!isLoading" class="project">
      <h3>
        {{ toDisplay[0]?.title }}
      </h3>
      <img :src="toDisplay[0]?.image" alt="image projet" id="img_projet_principale"/>
      <p>
        {{ toDisplay[0].description.length < 150 ? toDisplay[0].description : toDisplay[0].description.split('.')[0] + '...' }}
      </p>
      <a :href="toDisplay[0]?.lien" target="_blank"> <img src="../../assets/github.svg" alt="github icon"></a>
      <button class="knowMore" @click="$emit('toggleProject')">Voir mes autres projets</button>
    </span>
  </div>
</template>

<style scoped>
div {
  position: fixed;

  bottom: 0.5em;
  left: 0.5em;
  padding: 1em;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width:30.2vw;
  height: 68vh;
}

div > h2 {
  margin: 0;
}

h3{
  margin-top: 0.5em;
  padding-top: 0.5em;
  margin-bottom: 0.5em;
}

.project{
  display: flex;
  height: 100%;
  flex-direction: column;
  align-items: center;
}

p {
  margin: 0;
  padding-bottom: 1em;
}

img[alt='github icon'] {
  filter: invert(100%) sepia(0%) saturate(7500%) hue-rotate(116deg) brightness(99%) contrast(102%);
  height: 3em;
}


#img_projet_principale{
  max-height: 50%;
  margin-bottom: 0.5em;
  padding-bottom: 1em;
  padding-top: 0.5em;
}

</style>