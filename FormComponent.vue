<template>
    <div class="q-pa-md bg-grey-4">
      <q-form @submit="onSubmit" @reset="onReset" class="q-col-gutter-x-md">
        <div class="row q-col-gutter-md">
          <q-select
            filled
            v-model="formType"
            :options="['Produto', 'Movimentação']"
            label="Tipo de Cadastro"
            class="col-6"
          />
        </div>
  
        <div v-if="formType === 'Produto'">
          <div class="row q-col-gutter-md">
            <q-input
              filled
              v-model="name"
              label="Nome do Produto"
              class="col-6"
            />
            <q-input
              filled
              v-model="description"
              label="Descrição"
              class="col-6"
            />
          </div>
        </div>
  
        <div v-else>
          <div class="row q-col-gutter-md">
            <q-select
              filled
              v-model="selectedProductId"
              :options="productOptions"
              label="Produto"
              class="col-6"
            />
            <q-select
              filled
              v-model="movementType"
              :options="['entrada', 'saida']"
              label="Tipo de Movimentação"
              class="col-6"
            />
            <q-input
              filled
              v-model="quantity"
              label="Quantidade"
              type="number"
              class="col-6"
            />
          </div>
        </div>
  
        <div class="q-mt-md">
          <q-btn label="Submit" type="submit" color="primary" />
          <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
        </div>
      </q-form>
    </div>
  </template>
  
  <script setup>
  import { ref, defineEmits, onMounted } from 'vue'
  import { addNewData, getLocalStorage } from 'src/utils/utils'
  
  const emit = defineEmits(['dataUpdated'])
  
  const formType = ref('Produto')
  const name = ref('')
  const description = ref('')
  const selectedProductId = ref(null)
  const movementType = ref('entrada')
  const quantity = ref(0)
  const productOptions = ref([])
  
  const onSubmit = () => {
    if (formType.value === 'Produto') {
      const data = {
        id: Date.now(),
        name: name.value,
        description: description.value,
      }
      addNewData('products', data)
    } else {
      const data = {
        id: Date.now(),
        produto_id: selectedProductId.value,
        tipo: movementType.value,
        quantidade: parseInt(quantity.value),
        data: new Date().toISOString(),
      }
      addNewData('movimentacoes', data)
    }
  
    emit('dataUpdated')
    onReset()
  }
  
  const onReset = () => {
    name.value = ''
    description.value = ''
    selectedProductId.value = null
    movementType.value = 'entrada'
    quantity.value = 0
  }
  
  onMounted(() => {
    const storedProducts = getLocalStorage('products') || []
    productOptions.value = storedProducts.map((product) => ({
      label: product.name,
      value: product.id,
    }))
  })
  </script>
  