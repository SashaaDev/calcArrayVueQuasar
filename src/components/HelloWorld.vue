<template>
  <q-page class="row justify-center items-center">
    <q-card class="row justify-center q-pa-md" style="max-width: 400px">
      <q-item class="q-pa-md wrap-sm">
        <q-item-section class="q-pa-md" style="min-width: 300px">
          <q-input class="q-mb-md" v-model="text" filled type="textarea" :label=label />
          <q-select class="q-mb-md" v-model="typeOperation" :options="options" label="Standard" style="max-width: 250px"/> <!-- Зменшено ширину селекта -->
          <q-input class="q-mb-md" filled v-model="result" hint="Readonly" readonly />
          <q-card-section class="row justify-center q-mb-md">
            <q-btn @click="request" label="Send" type="submit" color="primary" />
            <q-btn @click="text = ''; result = ''; errorMessage = ''; typeOperation = ''" label="Reset" type="reset" color="primary" flat class="q-ml-md" />
          </q-card-section>
          <q-item-label class="text-red row justify-center q-pa-md" v-if="errorMessage">{{ errorMessage }}</q-item-label>
        </q-item-section>
      </q-item>
    </q-card>
  </q-page>
</template>


<style>
</style>

<script>
export default {
  name: 'HelloWorld'
}
</script>
<script setup>
import {ref} from 'vue'

const options = ['min', 'max', 'sum']
const typeOperation = ref('')
const text = ref('')
const label = ref('Enter your array')
const result = ref('')
const errorMessage = ref('')
const request = () => {
  fetch(`http://localhost/api/test/${typeOperation.value}`, {
    method: 'POST',
    headers: {
      'Accept': 'application/json',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      'array': ifJSON(text.value)
    })
  })
      .then(response => {
        if (!response.ok) {
          throw new Error('Network response was not ok');
        } else {
          errorMessage.value = ''
        }
        return response.json();
      })
      .then(data => {
        result.value = data;
      })
      .catch(error => {
        if (typeOperation.value === '') {
          errorMessage.value = 'Select operation'
        } else {
          console.error('There was an error!', error);
          errorMessage.value = 'There was an error!'
        }
      });
}

function ifJSON(str) {
  try {
    return JSON.parse(str);
  } catch (e) {
    return str;
  }
}
</script>


