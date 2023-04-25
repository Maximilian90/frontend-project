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
  const day = parseInt(dateString.substring(6, 8)); 
  
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



</script>

<template>
  
  <div class="event_calender_list">
    <div class="list_head">
      <h2>April 2023</h2>
      <button @click="showForm = true">Opret Event</button>
    </div>
    <div class="event_calender_row" v-for="event in events" :key="event.id">
      <div class="event_calender_item">
        <div class="event_calender_date_tag">
          <p class="date_style" v-if="event">{{ event.plainDate }}</p> 
        </div>
        <div class="event_calender_content">
          <div class="event_detail">
            <p class="event_loc">{{ event.eventLocation }}</p>
            <h3>{{ event.title.rendered }}</h3>
            <p class="event_ex">{{ event.plainText }}</p>
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
  padding: 1rem;
  background-color: white;
}

.event_img {
  flex: 0 0 auto;
}

.img_list {
  width: 60vh;
  height: auto;
}

.btn_addevent {
  height: 35px;
  width: 100px; 
  
}

.list_head {
  display: flex;
  justify-content: space-between;  
}




/* styling */
h3 {
  color: black;
  font-size: 1.5em;
  font-weight: 600;
}

.date_style {
  text-align: center;
  font-size: 1.25em;
  font-weight: 600;
  padding: 1rem;
  color: black;
  background-color: rgb(255, 255, 255);
}

.event_loc {
  color: rgb(15, 15, 15);
  font-weight: 500;
  font-size: 1em;
}

.event_ex {
  color: black;
  font-size: 1em;
}



</style>