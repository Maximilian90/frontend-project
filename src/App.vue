<!--<script setup>
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
    title.value = response[0].title.rendered;
    content.value = response[0].content.rendered;
  });
</script>

<template>
  <div>
    <h3>Pros</h3>
      <ul>
        <li v-for="item in title">Single title goes here
          <h4>{{ title }}</h4>
          <p>{{ content }}</p>
        </li>
      </ul>
  </div>
</template>

<style scoped></style>
-->
<script setup>
import { ref } from 'vue';

const events = ref([]);

fetch('https://process.mg-visions.com/wp-json/wp/v2/events')
  .then(response => response.json())
  .then(data => events.value = data);
</script>

<template>
  <div>
    <h3>Pros</h3>
    <ul>
      <li v-for="event in events" :key="event.id">
        <h4>{{ event.title.rendered }}</h4>
        <p>{{ event.content.rendered }}</p>
      </li>
    </ul>
  </div>
</template>
