<template>
  <div v-if="productData.length > 0">
    <TitleIndexComponent />
    <MenuAdminComponent/>
    <MenuBrandComponent />
    <CardComponent :productData="productData" />
  </div>
  <div v-else>
    Carregando...
  </div>
</template>


<script setup lang="ts">
import { Todo, Meta } from 'components/models';
import CardComponent from 'src/components/CardComponent.vue';
import MenuBrandComponent from 'src/components/MenuBrandComponent.vue';
import TitleIndexComponent from 'src/components/TitleIndexComponent.vue';
import { ref, onMounted } from 'vue';
import api from "../axios/api";
import MenuAdminComponent from 'src/components/MenuAdminComponent.vue';

const productData = ref([]);

async function getProductAllData() {
  try {
    const responseData = await api.get('/api/applianceProduct');
    productData.value = responseData.data;
    console.log(productData.value[0])
  } catch (err) {
    console.error(err);
  }
}

onMounted(async () => {
  await getProductAllData();
});

const meta = ref<Meta>({
  totalCount: 1200,
});
</script>
