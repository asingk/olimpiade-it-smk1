<script setup>
import axios from "axios";

import { ref } from 'vue';

const products = ref([]);
function getProducts() {
  axios.get('https://dummyjson.com/products', {
    headers: {
      'Authorization': `Bearer ${localStorage.getItem("token")}`,
      'Content-Type': 'application/json'
    }
  })
    .then(function (response) {
      // handle success
      console.log(response.data.products);
      products.value = response.data.products
    })
    .catch(function (error) {
      // handle error
      console.log(error);
    })
    .finally(function () {
      // always executed
    });
}
getProducts()
</script>

<template>
  <VTable>
    <thead>
    <tr>
      <th class="text-uppercase">
        Title
      </th>
      <th></th>
    </tr>
    </thead>

    <tbody>
    <tr
      v-for="item in products"
      :key="item.id"
    >
      <td>
        {{ item.title }}
      </td>
      <td class="text-center">
        <VRow align="center" justify="center">
          <VCol cols="auto">
            <VBtn color="success" density="comfortable" icon="raphael:view" @click="$router.push('/products/' + item.id)"></VBtn>
          </VCol>
        </VRow>
      </td>
    </tr>
    </tbody>
  </VTable>
</template>
