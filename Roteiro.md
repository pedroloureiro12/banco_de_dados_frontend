#  Roteiro para a utilização do projeto

# 1 passo:
a instalação Node.js e NPM, entre no site do Node.js (https://nodejs.org/en) e baixe a versão recomendada (LTS) e siga o comando a baixo para realizar de forma correta

```bash
node -v
```
Se o node tiver a versão 22.9.0 é a correta. agora verificamos o gerenciador de pacotes npm com o comando baixo:
```bash
npm -v
```
Isso irá mostrar a versão do Node e NPM caso esteja instalado corretamente.

2° passo: Instalação e configuração do Quasar Framework

para a realização do projeto, é necessário instalar o Quasar CLI pelo seguinte comando

```bash
npm install -g @quasar/cli 
``` 

Para depois iniciamos o projeto com o comando no terminal:

```bash
npm init quasar@latest
````
3° Passo: Escolha de builds e configurações do projeto

Para conseguirmos executar o código, necessita de algumas definições escolhidas quando se inicia o projeto, que serão as 

```
APP with Quasar CLI, let's go!
Quasar APP CLI with Vite
CRUD simples
Composition API with <script setup>
Nome(the others Will still be avalable)
Linting( vite-plugin-checker + ESLint) vue-118n
yes, use npm
```
Para conseguimos modificar as nossas configurações definidas, aplicamos o seguintes comandos

```bash
cd nome_da_pasta
code .
``` 
4° passo: Criação do projeto CRUD

Para se criar o CRUD, será necessário algumas pequenas modificações e criações de arquivos e pastas, entre o primeiro será a remoção do 'EssentialLink.vue'. 

A remoção da linha de código do arquivo 'IndexPage.vue' que será:

```javascript
<img 
   alt= "Quasar logo"
   src = "~assets/quasar-logo-vertical.svg"
   styler = "with: 200px; height: 200px"
>
```
5°passo: Local Storage para guardar os dados

Criamos uma pasta e um arquivo no diretório src/utils sendo o arquivo com o nome utils.js, sendo nesse arquivo o seguinte código

```javascript
// Salva um value no localStorage com a chave key
function saveLocalStorage(key, value) {
    localStorage.setItem(key, JSON.stringify(value))
}

// Retorna o value salvo no localStorage com a chave key ou um array vazio caso não exista
function getLocalStorage(key) {
    return JSON.parse(localStorage.getItem(key)) || []
}

function addNewData(key, data) {
    // Busca os dados salvos no localStorage com a chave key
    const dataStorage = getLocalStorage(key)
    // Gera um id para o novo dado
    const id = dataStorage.length ? dataStorage[dataStorage.length - 1].id + 1 : 1
    // Cria um novo dado com o id gerado e os dados passados
    const newData = { id, ...data }
    // Adiciona o novo dado ao array de dados
    dataStorage.push(newData)
    // Salva o array de dados atualizado no localStorage
    saveLocalStorage(key, dataStorage)
}

function editData(key, id, data) {
    // Busca os dados salvos no localStorage com a chave key
    const dataStorage = getLocalStorage(key)
    // Busca o índice do dado que será editado
    const index = dataStorage.findIndex((item) => item.id === id)
    // Substitui o dado antigo pelo novo dado
    dataStorage[index] = { id, ...data }
    // Salva o array de dados atualizado no localStorage
    saveLocalStorage(key, dataStorage)
}

function deleteData(key, id) {
    // Busca os dados salvos no localStorage com a chave key
    const dataStorage = getLocalStorage(key)
    // Busca o índice do dado que será deletado
    const index = dataStorage.findIndex((item) => item.id === id)
    // Remove o dado do array
    dataStorage.splice(index, 1)
    // Salva o array de dados atualizado no localStorage
    saveLocalStorage(key, dataStorage)
}

export { savelocalStorage, getLocalStorage, addNewData, editData, deleteData}
```

6° passo:FormComponent.vue

O FormComponent.vue vai ser criado no src/components, sendo o responsável por ler e enviar os dados do usuário, o seu código é 

```vue

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
 ```
7° passo: TableComponent.vue

Criamos o TableComponent.vue no mesmo direitorio do código anterior, ele será responsável por mostrar os dados salvos. O código é: 

```vue
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
 ```
8° passo: IndexPage.vue

usaremos esse arquivo para unimos os dois componentes anteriores, seu código será: 

```vue
<script setup>
import FormComponent from 'src/components/FormComponent.vue'
import TableComponent from 'src/components/TableComponent.vue'
import { getLocalStorage } from 'src/utils/utils'
import { ref } from 'vue'
const rows = ref(getLocalStorage('dataCompany'))
// Busca os dados do localStorage e armazena em rows
const updateTableData = () => {
  rows.value = getLocalStorage('dataCompany')
  // Atualiza os dados da tabela com os dados do localStorage
}
</script>
<template>
  <q-page padding>
    <FormComponent @data-updated="updateTableData" />
    <TableComponent :rows="rows" />
  </q-page>
</template>
``` 
Por fim, depois de realizar todos esses passos, no terminal do vs code digitaremos para iniciamos o banco de dados.

```bash
Quasar Dev
```




