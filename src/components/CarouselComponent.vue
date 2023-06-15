<template>
  <div class="q-pa-md">
    <h5 class="brand-title">Selecione sua marca favorita</h5>
    <q-carousel
      v-model="slide"
      swipeable
      animated
      :control-type="controlType"
      control-color="primary"
      navigation
      padding
      arrows
      height="300px"
      class="text-primary rounded-borders"
      @mounted="advanceCarousel"
    >
      <q-carousel-slide
        v-for="(item, index) in slideItemBrand"
        :key="index"
        :name="`slide-${index}`"
        class="column no-wrap flex-center"
      >
        <q-icon :name="item.icon" size="56px" />
        <div class="q-mt-md text-center">
          {{ item.name }}
        </div>
      </q-carousel-slide>
    </q-carousel>
  </div>
</template>

<script setup lang="ts">
import { ref, onBeforeMount } from 'vue'
import api from "../axios/api";

const controlType = ref('flat');
const slide = ref(0);
const slideItemBrand = ref([]);

async function getBrandData() {
  try {
    const responseData = await api.get('/api/applianceBrand');
    slideItemBrand.value = responseData.data;
  } catch (err) {
    console.error(err);
  }
}

onBeforeMount(async () => {
  await getBrandData();
  advanceCarousel()
});

function advanceCarousel() {
  const lensIcon = document.querySelector('.q-icon.notranslate.material-icons') as HTMLElement;
  if (lensIcon) {
    lensIcon.click();
  }
}
</script>

<style lang="sass" scoped>
.custom-link 
  color: inherit
  text-decoration: none
  cursor: pointer

.my-card
  width: 100%
  max-width: 350px

.brand-title 
  font-size: 1.2rem
  font-weight: bold
  color: #333
  text-transform: uppercase
  margin-bottom: 0

</style>
