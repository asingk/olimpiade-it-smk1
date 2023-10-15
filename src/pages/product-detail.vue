<script setup>
import avatar1 from '@images/avatars/avatar-1.png'
import {ref} from "vue";
import axios from "axios";
import router from "@/router";

const accountData = {
  avatarImg: avatar1,
  firstName: 'john',
  lastName: 'Doe',
  email: 'johnDoe@example.com',
  org: 'ThemeSelection',
  phone: '+1 (917) 543-9876',
  address: '123 Main St, New York, NY 10001',
  state: 'New York',
  zip: '10001',
  country: 'USA',
  language: 'English',
  timezone: '(GMT-11:00) International Date Line West',
  currency: 'USD',
}

const refInputEl = ref()
const accountDataLocal = ref(structuredClone(accountData))
const isAccountDeactivated = ref(false)

const resetForm = () => {
  accountDataLocal.value = structuredClone(accountData)
}

const changeAvatar = file => {
  const fileReader = new FileReader()
  const { files } = file.target
  if (files && files.length) {
    fileReader.readAsDataURL(files[0])
    fileReader.onload = () => {
      if (typeof fileReader.result === 'string')
        accountDataLocal.value.avatarImg = fileReader.result
    }
  }
}

// reset avatar image
const resetAvatar = () => {
  accountDataLocal.value.avatarImg = accountData.avatarImg
}

const timezones = [
  '(GMT-11:00) International Date Line West',
  '(GMT-11:00) Midway Island',
  '(GMT-10:00) Hawaii',
  '(GMT-09:00) Alaska',
  '(GMT-08:00) Pacific Time (US & Canada)',
  '(GMT-08:00) Tijuana',
  '(GMT-07:00) Arizona',
  '(GMT-07:00) Chihuahua',
  '(GMT-07:00) La Paz',
  '(GMT-07:00) Mazatlan',
  '(GMT-07:00) Mountain Time (US & Canada)',
  '(GMT-06:00) Central America',
  '(GMT-06:00) Central Time (US & Canada)',
  '(GMT-06:00) Guadalajara',
  '(GMT-06:00) Mexico City',
  '(GMT-06:00) Monterrey',
  '(GMT-06:00) Saskatchewan',
  '(GMT-05:00) Bogota',
  '(GMT-05:00) Eastern Time (US & Canada)',
  '(GMT-05:00) Indiana (East)',
  '(GMT-05:00) Lima',
  '(GMT-05:00) Quito',
  '(GMT-04:00) Atlantic Time (Canada)',
  '(GMT-04:00) Caracas',
  '(GMT-04:00) La Paz',
  '(GMT-04:00) Santiago',
  '(GMT-03:30) Newfoundland',
  '(GMT-03:00) Brasilia',
  '(GMT-03:00) Buenos Aires',
  '(GMT-03:00) Georgetown',
  '(GMT-03:00) Greenland',
  '(GMT-02:00) Mid-Atlantic',
  '(GMT-01:00) Azores',
  '(GMT-01:00) Cape Verde Is.',
  '(GMT+00:00) Casablanca',
  '(GMT+00:00) Dublin',
  '(GMT+00:00) Edinburgh',
  '(GMT+00:00) Lisbon',
  '(GMT+00:00) London',
]

const currencies = [
  'USD',
  'EUR',
  'GBP',
  'AUD',
  'BRL',
  'CAD',
  'CNY',
  'CZK',
  'DKK',
  'HKD',
  'HUF',
  'INR',
]

const props = defineProps(['id'])
const product = ref([]);
function getProductDetail() {
  axios.get(`https://dummyjson.com/products/${props.id}`, {
    headers: {
      'Authorization': `Bearer ${localStorage.getItem("token")}`,
      'Content-Type': 'application/json'
    }
  })
    .then(function (response) {
      // handle success
      console.log(response.data);
      product.value = response.data
    })
    .catch(function (error) {
      // handle error
      console.log(error);
    })
    .finally(function () {
      // always executed
    });
}
getProductDetail()

const submitAction = () => {
  axios.put(`https://dummyjson.com/products/${props.id}`, {
    title: product.value.title,
    description: product.value.description,
    price: product.value.price,
    rating: product.value.rating,
    stock: product.value.stock,
    brand: product.value.brand,
    category: product.value.category
  }, {
    headers: {
      'Authorization': `Bearer ${localStorage.getItem("token")}`,
      'Content-Type': 'application/json'
    }
  })
    .then(function (response) {
      // handle success
      console.log(response.data);
    })
    .catch(function (error) {
      // handle error
      console.log(error);
    })
    .finally(function () {
      // always executed
    });
}

const dialog = ref(false)

const deleteAction = () => {
  axios.delete(`https://dummyjson.com/products/${props.id}`, {
    headers: {
      'Authorization': `Bearer ${localStorage.getItem("token")}`,
      'Content-Type': 'application/json'
    }
  })
    .then(function (response) {
      // handle success
      console.log(response.data);
      dialog.value = false
      router.push('/products')
    })
    .catch(function (error) {
      // handle error
      console.log(error);
    })
    .finally(function () {
      // always executed
    });
}
</script>

<template>
  <VRow>
    <VCol cols="12">
      <VCard title="Account Details">
        <VCardText class="d-flex">
          <!-- 👉 Avatar -->
          <VAvatar
            rounded="lg"
            size="100"
            class="me-6"
            :image="accountDataLocal.avatarImg"
          />

          <!-- 👉 Upload Photo -->
          <form class="d-flex flex-column justify-center gap-5">
            <div class="d-flex flex-wrap gap-2">
              <VBtn
                color="primary"
                @click="refInputEl?.click()"
              >
                <VIcon
                  icon="bx-cloud-upload"
                  class="d-sm-none"
                />
                <span class="d-none d-sm-block">Upload new photo</span>
              </VBtn>

              <input
                ref="refInputEl"
                type="file"
                name="file"
                accept=".jpeg,.png,.jpg,GIF"
                hidden
                @input="changeAvatar"
              >

              <VBtn
                type="reset"
                color="error"
                variant="tonal"
                @click="resetAvatar"
              >
                <span class="d-none d-sm-block">Reset</span>
                <VIcon
                  icon="bx-refresh"
                  class="d-sm-none"
                />
              </VBtn>
            </div>

            <p class="text-body-1 mb-0">
              Allowed JPG, GIF or PNG. Max size of 800K
            </p>
          </form>
        </VCardText>

        <VDivider />

        <VCardText>
          <!-- 👉 Form -->
          <VForm class="mt-6">
            <VRow>
              <!-- 👉 First Name -->
              <VCol
                md="6"
                cols="12"
              >
                <VTextField
                  v-model="product.title"
                  placeholder="John"
                  label="Title"
                />
              </VCol>

              <!-- 👉 Last Name -->
              <VCol
                md="6"
                cols="12"
              >
                <VTextField
                  v-model="product.description"
                  placeholder="Doe"
                  label="Description"
                />
              </VCol>

              <!-- 👉 Email -->
              <VCol
                cols="12"
                md="6"
              >
                <VTextField
                  v-model="product.price"
                  label="Price"
                  placeholder="johndoe@gmail.com"
                />
              </VCol>

              <!-- 👉 Organization -->
              <VCol
                cols="12"
                md="6"
              >
                <VTextField
                  v-model="product.discountPercentage"
                  label="Discount(%)"
                  placeholder="ThemeSelection"
                />
              </VCol>

              <!-- 👉 Phone -->
              <VCol
                cols="12"
                md="6"
              >
                <VTextField
                  v-model="product.rating"
                  label="Rating"
                  placeholder="+1 (917) 543-9876"
                />
              </VCol>

              <!-- 👉 Address -->
              <VCol
                cols="12"
                md="6"
              >
                <VTextField
                  v-model="product.stock"
                  label="Stock"
                  placeholder="123 Main St, New York, NY 10001"
                />
              </VCol>

              <!-- 👉 State -->
              <VCol
                cols="12"
                md="6"
              >
                <VTextField
                  v-model="product.brand"
                  label="Brand"
                  placeholder="New York"
                />
              </VCol>

              <!-- 👉 Zip Code -->
              <VCol
                cols="12"
                md="6"
              >
                <VTextField
                  v-model="product.category"
                  label="Category"
                  placeholder="10001"
                />
              </VCol>

              <!-- 👉 Country -->
              <VCol
                cols="12"
                md="6"
              >
                <VSelect
                  v-model="accountDataLocal.country"
                  label="Country"
                  :items="['USA', 'Canada', 'UK', 'India', 'Australia']"
                  placeholder="Select Country"
                />
              </VCol>

              <!-- 👉 Language -->
              <VCol
                cols="12"
                md="6"
              >
                <VSelect
                  v-model="accountDataLocal.language"
                  label="Language"
                  placeholder="Select Language"
                  :items="['English', 'Spanish', 'Arabic', 'Hindi', 'Urdu']"
                />
              </VCol>

              <!-- 👉 Timezone -->
              <VCol
                cols="12"
                md="6"
              >
                <VSelect
                  v-model="accountDataLocal.timezone"
                  label="Timezone"
                  placeholder="Select Timezone"
                  :items="timezones"
                  :menu-props="{ maxHeight: 200 }"
                />
              </VCol>

              <!-- 👉 Currency -->
              <VCol
                cols="12"
                md="6"
              >
                <VSelect
                  v-model="accountDataLocal.currency"
                  label="Currency"
                  placeholder="Select Currency"
                  :items="currencies"
                  :menu-props="{ maxHeight: 200 }"
                />
              </VCol>

              <!-- 👉 Form Actions -->
              <VCol
                cols="12"
                class="d-flex flex-wrap gap-4"
                type="submit"
                @click.prevent="submitAction"
              >
                <VBtn>Save changes</VBtn>

                <VBtn
                  color="secondary"
                  variant="tonal"
                >
                  Delete
                  <VDialog
                    v-model="dialog"
                    activator="parent"
                    width="auto"
                  >
                    <VCard>
                      <VCardText>
                        Anda yakin ingi menghapus produk ini?
                      </VCardText>
                      <VCardActions>
                        <VSpacer></VSpacer>
                        <VBtn
                          color="green-darken-1"
                          variant="text"
                          @click="dialog = false"
                        >
                          Tidak
                        </VBtn>
                        <VBtn
                          color="green-darken-1"
                          variant="text"
                          @click="deleteAction"
                        >
                          Ya
                        </VBtn>
                      </VCardActions>
                    </VCard>
                  </VDialog>
                </VBtn>
              </VCol>
            </VRow>
          </VForm>
        </VCardText>
      </VCard>
    </VCol>
  </VRow>
</template>
