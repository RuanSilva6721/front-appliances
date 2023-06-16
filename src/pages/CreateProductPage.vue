<template>
  <q-page>
    <q-card class="q-pa-md">
      <q-card-section class="q-mb-md">
        <h2 class="text-h6">Criar Produto</h2>
        <q-form @submit="submitForm">
          <q-input
            v-model="productData.name"
            label="Nome"
            outlined
            required
          ></q-input>
          <q-input
            class="q-mt-sm"
            v-model="productData.description"
            label="Descrição"
            outlined
            required
          ></q-input>
          <q-select
            class="q-mt-sm"
            v-model="productData.voltage"
            label="Voltagem"
            outlined
            :options="voltageOptions"
            required
          ></q-select>
          <q-select
            class="q-mt-sm"
            v-model="productData.brand_id"
            label="ID da Marca"
            outlined
            :options="brandOptions"
            required
          ></q-select>
          <q-btn class="q-mt-sm" type="submit" label="Criar Produto" color="primary"></q-btn>
        </q-form>
      </q-card-section>
    </q-card>

    <q-dialog v-model="modalOpen">
      <q-card>
        <q-card-section>
          <div class="text-h6" :class="modalType === 'success' ? 'text-green' : 'text-red'">
            {{ modalType === 'success' ? 'Sucesso!' : 'Falha!' }}
          </div>
          <div>{{ modalMessage }}</div>
        </q-card-section>

        <q-card-actions align="right">
          <q-btn label="Fechar" color="primary" @click="modalOpen = false" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import api from "../axios/api";

const modalType = ref();
const modalMessage = ref();
const modalOpen = ref();
const productData = ref({
  id: 1,
  name: '',
  description: '',
  voltage: '110v',
  brand_id: '',
});

const voltageOptions = ref([
  { value: '110v', label: '110v' },
  { value: '220v', label: '220v' },
]);

const brandOptions = ref();



async function getBrandAllData() {
  try {
    const responseData = await api.get('/api/applianceBrand');
    brandOptions.value = responseData.data.map((brand) => ({
      value: brand.id,
      label: brand.name,
    }));
  } catch (err) {
    console.error(err);
  }
}

onMounted(async () => {
  await getBrandAllData();
});

const showModal=  (type, message) => {
    modalType.value = type;
    modalMessage.value = message;
    modalOpen.value = true;
}

async function submitForm() {
  try {
    const responseData = await api.post(`/api/applianceProductCreate`, {
      description: productData.value.description,
      name: productData.value.name,
      voltage: productData.value.voltage.value,
      brand_id: productData.value.brand_id.value
    });
    if(responseData.data.success = true){
      console.log(responseData.data.message);
    }
    showModal('error', 'Falha ao tentar criar o produto!');
    showModal('success', responseData.data.message);
  } catch (err) {
    showModal('error', err.response.data.message);
  }
}
</script>
