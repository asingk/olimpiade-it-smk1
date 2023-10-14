<script setup>
import axios from "axios";

const desserts = [
  {
    dessert: 'Frozen Yogurt',
    calories: 159,
    fat: 6,
    carbs: 24,
    protein: 4,
  },
  {
    dessert: 'Ice cream sandwich',
    calories: 237,
    fat: 6,
    carbs: 24,
    protein: 4,
  },
  {
    dessert: 'Eclair',
    calories: 262,
    fat: 6,
    carbs: 24,
    protein: 4,
  },
  {
    dessert: 'Cupcake',
    calories: 305,
    fat: 6,
    carbs: 24,
    protein: 4,
  },
  {
    dessert: 'Gingerbread',
    calories: 356,
    fat: 6,
    carbs: 24,
    protein: 4,
  },
]
import { ref } from 'vue';

const products = ref([]);
async function getProducts() {
  axios.get('https://dummyjson.com/products')
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
      <th>
        description
      </th>
      <th>
        price
      </th>
      <th>
        discount(%)
      </th>
      <th>
        rating
      </th>
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
        {{ item.description }}
      </td>
      <td class="text-center">
        {{ item.price }}
      </td>
      <td class="text-center">
        {{ item.discountPercentage }}
      </td>
      <td class="text-center">
        {{ item.rating }}
      </td>
    </tr>
    </tbody>
  </VTable>
</template>
