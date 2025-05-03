<template>
    <div class="q-pa-md">
      <FormComponent @dataUpdated="refreshData" />
  
      <div class="q-mt-lg">
        <h5>Produtos</h5>
        <q-table
          title="Produto"
          :rows="products"
          :columns="productColumns"
          row-key="id"
        >
          <template v-slot:body="props">
            <q-tr :props="props">
              <q-td v-for="col in productColumns" :key="col.name" :props="props">
                <div v-if="col.name === 'actions'">
                  <q-btn color="negative" label="excluir" @click="handleDelete('products', props.row.id)" />
                </div>
                <div v-else>
                  <div v-if="col.editable">
                    {{ props.row[col.field] }}
                    <q-popup-edit v-model="props.row[col.field]" v-slot="scope">
                      <q-input v-model="scope.value" dense autofocus @keyup.enter="editar('products', props.row.id, col.field, scope.value, scope)" />
                    </q-popup-edit>
                  </div>
                  <div v-else>
                    {{ props.row[col.field] }}
                  </div>
                </div>
              </q-td>
            </q-tr>
          </template>
        </q-table>
      </div>
  
      <div class="q-mt-xl">
        <h5>Movimentações</h5>
        <q-table
          title="movimentacao"
          :rows="movements"
          :columns="movementColumns"
          row-key="id"
        >
          <template v-slot:body="props">
            <q-tr :props="props">
              <q-td v-for="col in movementColumns" :key="col.name" :props="props">
                <div v-if="col.name === 'actions'">
                  <q-btn color="negative" label="Excluir" @click="handleDelete('movimentacoes', props.row.id)" />
                </div>
                <div v-else>
                  {{ props.row[col.field] }}
                </div>
              </q-td>
            </q-tr>
          </template>
        </q-table>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import { getLocalStorage, deleteData } from 'src/utils/utils.js'
  import FormComponent from 'src/components/FormComponent.vue'
  
  const products = ref([])
  const movements = ref([])
  
  const productColumns = [
    { name: 'id', label: 'id', field: 'id' },
    { name: 'name', label: 'nome', field: 'name', editable: true },
    { name: 'description', label: 'descricao', field: 'description', editable: true },
    { name: 'actions', label: 'acoes', field: 'actions' }
  ]
  
  const movementColumns = [
    { name: 'id', label: 'id', field: 'id' },
    { name: 'produto_id', label: 'id do produto', field: 'produto_id' },
    { name: 'tipo', label: 'tipo', field: 'tipo' },
    { name: 'quantidade', label: 'quantidade', field: 'quantidade' },
    { name: 'data', label: 'data', field: 'data' },
    { name: 'actions', label: 'acoes', field: 'actions' }
  ]
  
  const refreshData = () => {
    products.value = getLocalStorage('products') || []
    movements.value = getLocalStorage('movimentacoes') || []
  }
  
  const editar = (key, id, field, value, scope) => {
    const list = key === 'products' ? products : movements
    const index = list.value.findIndex((row) => row.id === id)
    if (index !== -1) {
      list.value[index][field] = value
      localStorage.setItem(key, JSON.stringify(list.value))
    }
    scope.set()
  }
  
  const handleDelete = (key, id) => {
    deleteData(key, id)
    refreshData()
  }
  
  onMounted(refreshData)
  </script>
  
