<template>
  <div class="q-pa-md">
    <h5 class="brand-title">Selecione sua marca favorita</h5>
    <div class="admin-menu">
      <a
        v-for="(item, index) in slideItemBrand"
        :key="index"
        :href="`/BrandProdutoList/${item.id}`"
        class="admin-menu-item custom-link"
      >
        <q-icon :name="item.icon" size="56px" />
        <div class="q-mt-md text-center">
          {{ item.name }}
        </div>
      </a>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onBeforeMount } from 'vue'
import api from "../axios/api";

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
});
</script>

<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 350px

.brand-title 
  font-size: 1.2rem
  font-weight: bold
  color: #333
  text-transform: uppercase
  margin-bottom: 0
  margin-left: 60px

.admin-menu
  display: flex
  flex-wrap: wrap
  justify-content: center
  gap: 16px
  margin-bottom: 60px
  margin-top: 60px

.admin-menu-item
  display: flex
  flex-direction: column
  align-items: center
  cursor: pointer

  .q-icon
    margin-bottom: 8px

.custom-link 
  color: inherit
  text-decoration: none
  cursor: pointer
</style>
