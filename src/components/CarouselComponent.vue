<template>
 <div class="q-pa-md">

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
    >
      <q-carousel-slide name="style" class="column no-wrap flex-center">
        <q-icon name="style" size="56px" />
        <div class="q-mt-md text-center">
          {{ lorem }}
        </div>
      </q-carousel-slide>
      <q-carousel-slide name="tv" class="column no-wrap flex-center">
        <q-icon name="live_tv" size="56px" />
        <div class="q-mt-md text-center">
          {{ lorem }}
        </div>
      </q-carousel-slide>
      <q-carousel-slide name="layers" class="column no-wrap flex-center">
        <q-icon name="layers" size="56px" />
        <div class="q-mt-md text-center">
          {{ lorem }}
        </div>
      </q-carousel-slide>
      <q-carousel-slide name="map" class="column no-wrap flex-center">
        <q-icon name="terrain" size="56px" />
        <div class="q-mt-md text-center">
          {{ lorem }}
        </div>
      </q-carousel-slide>
    </q-carousel>
  </div>
</template>
  
<script setup lang="ts">
import { ref } from 'vue'

    const controlType= ref('flat');
    const controlTypeOptions = [
        { value: 'regular', label: 'regular' },
        { value: 'unelevated', label: 'unelevated' },
        { value: 'flat', label: 'flat (default)' },
        { value: 'outline', label: 'outline' },
        { value: 'push', label: 'push' }
    ];

    const slide= ref('style');
    const lorem = 'Lorem ipsum dolor, sit amet consectetur adipisicing elit. Itaque1 voluptatem totam, architecto cupiditate officia rerum, error dignissimos praesentium libero ab nemo.';


</script>
<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 350px
</style>  