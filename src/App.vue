<script setup>
import { ref } from 'vue';
import CustomField from './components/CustomField.vue';

const events = ref([]);

fetch('https://process.mg-visions.com/wp-json/wp/v2/events')
  .then(response => response.json())
  .then(data => {
    events.value = data.map(event => {
      const customFieldValue = event.meta.yourprefix_demo_datepicker ? event.meta.yourprefix_demo_datepicker[0] : '';
      return { ...event, customFieldValue };
    });
  });

function getFeaturedImageUrl(event) {
  const content = event.content.rendered;
  const match = content.match(/<img[^>]+src="([^">]+)"/);
  if (match) {
    return match[1];
  } else {
    return '';
  }
}
</script>


<template>
  <div>
    <h3>Pros</h3>
    <ul>
      <li v-for="event in events" :key="event.id">
        <h4>{{ event.title.rendered }}</h4>
        <CustomField :customFieldValue="event.customFieldValue" />
        <img :src="getFeaturedImageUrl(event)" :alt="event.title.rendered" />
        <p>{{ event.content.rendered }}</p>
      </li>
    </ul>
  </div>
</template>