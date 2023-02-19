<script setup>
import { ref, onMounted, watch } from 'vue';


const headers = ref([
  { text: 'Nom', value: 'title' }
])
const items = ref([])
const serverOptions = ref({
  page: 1,
  rowsPerPage: 5
})
const serverItemsLength = ref(0)
const loading = ref(false)


function getData() {
  loading.value = true
  let limit = serverOptions.value.rowsPerPage
  let skip = (serverOptions.value.page - 1) * serverOptions.value.rowsPerPage
  fetch('https://jsonplaceholder.typicode.com/posts?_page=' + serverOptions.value.page + '&_limit=' + serverOptions.value.rowsPerPage).then(response => {
    return response.json()
  }).then(result => {
    console.log(result)
    items.value = result
    serverItemsLength.value = 50
  })
  loading.value = false

}
onMounted(() => {
  getData()
})
watch(serverOptions,
  async (newValue) => {
    if (newValue) {
      getData()
      console.log(serverOptions.value)
    }
  })
</script>

<template>
  <main>
    <EasyDataTable :headers="headers" :items="items" :buttons-pagination="true" v-model:server-options="serverOptions"
      :server-items-length="serverItemsLength" :loading="loading" :current-page=serverOptions.page>
    </EasyDataTable>
  </main>
</template>

<style></style>
