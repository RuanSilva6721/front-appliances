<template>
    <TitleIndexComponent />
    <MenuAdminComponent/>
    <h5 class="title">Marca Selecionada</h5>
      <MenuBrandComponent :slideItemBrand="slideItemBrand" />
    <div v-if="productData.length > 0">
      <h5 class="title">Produtos Da Marca Selecionada</h5>
        <CardComponent :productData="productData" />
    </div>
    <div v-else>
      Não há Produtos Cadastrados
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

const brandId = window.location.href.split('/').pop();
const productData = ref([]);
const slideItemBrand =  ref();
async function getProductAllData() {
  try {
    const responseData = await api.get(`/api/getProductOfBrand/${brandId}`);
    productData.value = responseData.data;
    console.log(responseData.data);
  } catch (err) {
    console.error(err);
  }
}
async function getBrandData() {
  try {
    const responseData = await api.get(`/api/applianceBrand/${brandId}`);
    slideItemBrand.value = [responseData.data];
    console.log(slideItemBrand.value)
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