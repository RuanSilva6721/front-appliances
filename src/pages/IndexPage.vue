<template>
 
    <TitleIndexComponent />
    <MenuAdminComponent/>
  <div v-if="productData.length > 0">
    <h5 class="title">Selecione sua marca favorita</h5>
      <MenuBrandComponent :slideItemBrand="slideItemBrand" />
    <h5 class="title">Produtos</h5>
      <CardComponent :productData="productData" />
  </div>
  <div v-else>
    Não há Marcas e Produtos cadastrados!!
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
const slideItemBrand =  ref([]);
async function getProductAllData() {
  try {
    const responseData = await api.get('/api/applianceProduct');
    productData.value = responseData.data;
  } catch (err) {
    console.error(err);
  }
}
async function getBrandData() {
  try {
    const responseData = await api.get('/api/applianceBrand');
    slideItemBrand.value = responseData.data;
  } catch (err) {
    console.error(err);
  }
}

onMounted(async () => {
  await getProductAllData();
  await getBrandData();
});

const meta = ref<Meta>({
  totalCount: 1200,
});
</script>
<style lang="sass" scoped>

.title
  font-size: 1.2rem
  font-weight: bold
  color: #333
  text-transform: uppercase
  margin-bottom: 0
  margin-left: 70px
</style>