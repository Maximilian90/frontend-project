<script setup>
import { ref } from 'vue';

const title = ref([]);
const content = ref([]);
/*const thumbnail = ref('Event img')
const pros = ref([]);
const cons = ref([]);*/


fetch('https://process.mg-visions.com/wp-json/wp/v2/events', {
  method: 'get',
})
  .then((response) => {
    return response.json();
  })
  .then((response) => {
    console.log(response);
    title.value = response.data.title;
    content.value = response.data.content;
    
  });
</script>

<template>
  <div>
    <h3>Pros</h3>
      <ul>
        <li v-for="item in title">Single title goes here
          {{ item }}
        </li>
      </ul>
  </div>
</template>

<style scoped></style>


