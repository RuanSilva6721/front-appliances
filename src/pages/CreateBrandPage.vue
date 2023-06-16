<template>
  <q-page>
    <q-card class="q-pa-md">
      <q-card-section class="q-mb-md">
        <h2 class="text-h6">Criar Produto</h2>
        <q-form @submit="submitForm">
          <q-input
            v-model="brandData.name"
            label="Nome"
            outlined
            required
          ></q-input>
          <q-btn class="q-mt-sm" type="submit" label="Criar Marca" color="primary"></q-btn>
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
import { ref } from 'vue';
import api from "../axios/api";

const modalType = ref();
const modalMessage = ref();
const modalOpen = ref();
const brandData = ref({
  name: ''
});

const showModal=  (type, message) => {
    modalType.value = type;
    modalMessage.value = message;
    modalOpen.value = true;
}

async function submitForm() {
  try {
    const responseData = await api.post(`/api/applianceBrandCreate`, {
      name: brandData.value.name,
      icon: 'live_tv'
    });
    if(responseData.data.success = true){
      console.log(responseData.data.message);
    }
    showModal('error', 'Falha ao tentar criar a marca!');
    showModal('success', responseData.data.message);
  } catch (err) {
    showModal('error', err.response.data.message);
  }
}
</script>
