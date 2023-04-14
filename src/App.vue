<script setup>
import { ref } from 'vue';

const title = ref('Title');
const content = ref('My content');
const pros = ref([]);
const cons = ref([]);

fetch('https://mock-api.h.kjc.dk/example',{
  method: 'get',
})
  .then((response) => {
    return response.json();
  })
  .then((response)  => {
    console.log(response);
    title.value = response.data.title;
    content.value = response.data.content;
    pros.value = response.data.pros;
    cons.value = response.data.cons;
  });
</script>

<template>
  <div>
    <h2>{{ title }}</h2>
    <p>{{ content }}</p>
    <hr>
    <h3>Pros</h3>
    <ul>
      <li v-for="item in pros">Single pro goes here
      {{ item }}
      </li>
    </ul>

    <h3>Cons</h3>
    <ul>
      <li v-for="item in cons">Single con goes here
      {{ item }}
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
