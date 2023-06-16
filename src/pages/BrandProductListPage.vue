<template>
  <TitleIndexComponent />
  <div class="q-pa-md row items-start q-gutter-md">
    <q-card class="my-card">
      <q-card-section class="bg-primary text-white">
        <div class="text-h6" @click="deleteBrand()">Excluir Marca</div>
      </q-card-section>
    </q-card>

    <q-card class="my-card">
      <q-card-section class="bg-primary text-white">
        <div class="text-h6" @click="openEditModal()">Editar Marca</div>
      </q-card-section>
    </q-card>
  </div>

  <!-- Modal -->
  <q-dialog v-model="showModal" @hide="resetModal">
    <q-card>
      <q-card-section>
        <h5 class="modal-title">{{ modalTitle }}</h5>
        <q-input v-model="modalValue" />
      </q-card-section>
      <q-card-actions align="right">
        <q-btn color="primary" label="Salvar" @click="saveModalValue" />
        <q-btn color="negative" label="Cancelar" @click="closeModal" />
      </q-card-actions>
    </q-card>
  </q-dialog>

  <!-- Success/Failure Message Modal -->
  <q-dialog v-model="showMessageModal" @hide="resetMessageModal">
    <q-card>
      <q-card-section>
        <h5 class="message-title">{{ message }}</h5>
      </q-card-section>
      <q-card-actions align="right">
        <q-btn color="primary" label="Fechar" @click="closeMessageModal" />
      </q-card-actions>
    </q-card>
  </q-dialog>

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
import { ref, onMounted } from 'vue';
import api from "../axios/api";
import CardComponent from 'src/components/CardComponent.vue';
import MenuBrandComponent from 'src/components/MenuBrandComponent.vue';
import TitleIndexComponent from 'src/components/TitleIndexComponent.vue';
import { useRouter } from 'vue-router';

const brandId = window.location.href.split('/').pop();
const productData = ref([]);
const slideItemBrand = ref();
const showModal = ref(false);
const showMessageModal = ref(false);
const modalTitle = ref('');
const modalValue = ref('');
const message = ref('');
const router = useRouter();

async function getProductAllData() {
  try {
    const responseData = await api.get(`/api/applianceProductOfBrand/${brandId}`);
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
  } catch (err) {
    console.error(err);
  }
}

function openEditModal() {
  showModal.value = true;
  modalTitle.value = 'Editar Marca';
  modalValue.value = slideItemBrand.value[0].name;
}

async function saveModalValue() {
  try {
    const responseData = await api.put(`/api/applianceBrand/${brandId}`, {
      name: modalValue.value,
    });
    showMessageModal.value = true;
    message.value = responseData.data.message;
    window.location.reload();
  } catch (err) {
    console.error(err);
    message.value = err.response.data.message;
  }
  closeModal();
}

function closeModal() {
  showModal.value = false;
}

function resetModal() {
  modalTitle.value = '';
  modalValue.value = '';
}

function closeMessageModal() {
  showMessageModal.value = false;
}

function resetMessageModal() {
  message.value = '';
}

async function deleteBrand() {
  try {
    const responseData = await api.delete(`/api/applianceBrand/${brandId}`);
    showMessageModal.value = true;
    message.value = responseData.data.message;
    router.push('/');
  } catch (err) {
    console.error(err);
    message.value = err.response.data.message;
  }
}

onMounted(async () => {
  await getProductAllData();
  await getBrandData();
});

const meta = ref({
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

/* Modal Styles */
.modal-title
  font-size: 1.2rem
  font-weight: bold
  margin-bottom: 8px

.message-title
  font-size: 1.2rem
  font-weight: bold
  margin-bottom: 8px

.q-card-actions
  justify-content: flex-end
</style>
