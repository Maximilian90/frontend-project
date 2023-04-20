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

fetch('https://process.mg-visions.com/wp-json/wp/v2/events')
  .then(response => response.json())
  .then(data => {
    events.value = data.map(event => {
      const customFieldValue = event.meta.yourprefix_demo_datepicker ? event.meta.yourprefix_demo_datepicker[0] : '';
      const plainText = getPlainText(event.content.rendered);
      return { ...event, customFieldValue, plainText };
    });
  });
</script>

<template>
  <div>
    <h3>Events</h3>
    <div class="flex_container_cal_list">
      <div class="date_box" v-for="event in events" :key="event.id">
        <CustomField :customFieldValue="event.customFieldValue" />
      </div>
      <div class="content_box" v-for="event in events" :key="event.id">
        <h4>{{ event.title.rendered }}</h4>
        <p>{{ event.plainText }}</p>
      </div>
      <div class="img_box" v-for="event in events" :key="event.id">
        <img class="img_list" :src="getFeaturedImageUrl(event)" :alt="event.title.rendered" />
      </div>
    </div>
  </div>
</template>

<style>

.flex_container_cal_list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.date_box {
  background-color: white;
  width: calc(33.33% - 10px);
  margin-right: 10px;
  padding: 10px;
}

.content_box {
  background-color: white;
  width: calc(33.33% - 10px);
  margin-right: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 10px;
}

.text_box {
  width: 50%;
}

.img_box {
  width: calc(33.33% - 10px);
  margin-left: 10px;
  text-align: center;
}

.img_list {
  max-width: 100%;
  height: auto;
}

/* Media queries */
@media only screen and (max-width: 768px) {
  .date_box,
  .content_box,
  .img_box {
    width: calc(50% - 10px);
    margin-right: 10px;
    margin-bottom: 20px;
  }

  .img_box {
    margin-left: 0;
  }

  .text_box {
    width: 60%;
  }
}

@media only screen and (max-width: 480px) {
  .flex_container_cal_list {
    flex-direction: column;
    align-items: center;
  }

  .date_box,
  .content_box,
  .img_box {
    width: 100%;
    margin-right: 0;
    margin-bottom: 20px;
  }

  .text_box {
    width: 100%;
    margin-bottom: 10px;
  }
}
</style>