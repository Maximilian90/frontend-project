<script setup>
import { ref } from 'vue';
import OpretForm from './components/OpretForm.vue';


const events = ref([]);
const showForm = ref(false);


const getPlainText = (content) => {
  const parser = new DOMParser();
  const html = parser.parseFromString(content, 'text/html');
  const firstParagraph = html.querySelector('p');
  return firstParagraph.textContent;
};

const getPlainDate = (dateString) => {
  const year = dateString.substring(0, 4);
  const monthNumber = parseInt(dateString.substring(4, 6)) - 1; 
  const day = parseInt(dateString.substring(6, 8)); r
  
  const monthNames = [
    "January", "February", "March",
    "April", "May", "June", "July",
    "August", "September", "October",
    "November", "December"
  ];
  
  const monthName = monthNames[monthNumber];
  
  return `${day} ${monthName}`;
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
      const plainDate = getPlainDate(event.acf.event_date);
      const plainText = getPlainText(event.content.rendered);
      return { ...event, plainText, plainDate, eventLocation: event.acf.location };
    });
  });

/*<my-component title="Hello" message="World!" />*/

</script>

<template>
  <button @click="showForm = true">Opret Event</button>
  <div class="event_calender_list">
    <div>
      <h2>April 2023</h2>
    </div>
    <div class="event_calender_row" v-for="event in events" :key="event.id">
      <div class="event_calender_item">
        <div class="event_calender_date_tag">
          <p class="date_style" v-if="event">{{ event.plainDate }}</p> 
        </div>
        <div class="event_calender_content">
          <div class="event_detail">
            <p>{{ event.eventLocation }}</p>
            <h3>{{ event.title.rendered }}</h3>
            <p>{{ event.plainText }}</p>
          </div>
          <div class="event_img">
            <img class="img_list" :src="getFeaturedImageUrl(event)" :alt="event.title.rendered" />
          </div>
        </div>
      </div>
    </div>
  </div>

  <teleport to="body">
    <div v-if="showForm">
      <OpretForm />
    </div>
  </teleport>
</template>

<style scoped>
.event_calender_row {
  display: flex;
  flex-wrap: wrap;
}

.event_calender_item {
  display: flex;
  align-items: center;
  margin: 1rem 1rem 1rem 1rem;
  background-color: rgb(0, 0, 0);
}

.event_calender_date_tag {
  flex: 0 auto;
  margin: 1rem;
  justify-content:first baseline;
}

.event_calender_content {
  display: flex;
  align-items: center;
}

.event_detail {
  flex: 1 1 auto;
  margin-right: 2rem;
}

.event_img {
  flex: 0 0 200px;
}

.btn_addevent {
  float: right;
  height: 35px;
  width: 100px;
}

h3 {
  font-size: 20px;
  font-weight: 600;
}

.date_style {
  font-size: 20px;
}

</style>