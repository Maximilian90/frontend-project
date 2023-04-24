<script setup>
import { ref } from 'vue';
import CustomField from './components/CustomField.vue';

const events = ref([]);

const getPlainText = (content) => {
  const parser = new DOMParser();
  const html = parser.parseFromString(content, 'text/html');
  const firstParagraph = html.querySelector('p');
  return firstParagraph.textContent;
};

const getFeaturedImageUrl = (event) => {
  const content = event.content.rendered;
  const match = content.match(/<img[^>]+src="([^">]+)"/);
  if (match) {
    return match[1];
  } else {
    return '';
  }
};

fetch('https://sesh.mg-visions.com/index.php/wp-json/wp/v2/event')
  .then(response => response.json())
  .then(data => {
    events.value = data.map(event => {
      const customFieldValue = event.meta.yourprefix_demo_textdate ? event.meta.yourprefix_demo_textdate[0] : '';
      const plainText = getPlainText(event.content.rendered);
      return { ...event, customFieldValue, plainText };
    });
  });
</script>

<template>
  <div class="event_calender_list">
    <h2>April 2023</h2>
    <div class="event_calender_row" v-for="event in events" :key="event.id">
      <div class="event_calender_item">
        <div class="event_calender_date_tag">{{ event.customFieldValue }} <p>date</p>
        </div>
        <div class="event_calender_content">
          <div class="event_detail">
            <h4>{{ event.title.rendered }}</h4>
            <p>{{ event.plainText }}</p>
          </div>
          <div class="event_img">
            <img class="img_list" :src="getFeaturedImageUrl(event)" :alt="event.title.rendered" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.event_calender_row {
  display: flex;
  flex-wrap: wrap;
}

.event_calender_item {
  display: flex;
  align-items: center;
  margin: 0 1rem 1rem 0;
}

.event_calender_date_tag {
  flex: 0 0 auto;
  margin-right: 1rem;
}

.event_calender_content {
  display: flex;
  align-items: center;
}

.event_detail {
  flex: 1 1 auto;
  margin-right: 1rem;
}

.event_img {
  flex: 0 0 200px;
}
</style>