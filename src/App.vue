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
/*
function getFeaturedImageUrl(event) {
  if (event._embedded && event._embedded['wp:featuredmedia'] && event._embedded['wp:featuredmedia'][0].source_url) {
    return event._embedded['wp:featuredmedia'][0].source_url;
  } else {
    return '';
  }
}
*/
</script>

<template>
  <div>
    <h3>Pros</h3>
    <ul>
      <li v-for="event in events" :key="event.id">
        <h4>{{ event.title.rendered }}</h4>
        <!--<img :src="getFeaturedImageUrl(event)" :alt="event.title.rendered" />-->
        <p>{{ event.content.rendered }}</p>
      </li>
    </ul>
  </div>
</template>
