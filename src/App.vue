<script setup>
import { ref } from 'vue';
import OpretForm from './components/OpretForm.vue';
import SeshFooter from './components/SeshFooter.vue';

const events = ref([]);
const showForm = ref(false);

const getPlainText = (content) => {
  const parser = new DOMParser();
  const html = parser.parseFromString(content, 'text/html');
  const firstParagraph = html.querySelector('p');
  const plainText = firstParagraph.textContent;
  const noHtmlText = plainText.replace(/<(?:.|\n)*?>/gm, '');
  return noHtmlText;
};


const getPlainDate = (dateString) => {
  const year = dateString.substring(0, 4);
  const monthNumber = parseInt(dateString.substring(4, 6)) - 1; 
  const day = parseInt(dateString.substring(6, 8)); 
  
  const monthNames = [
    "Januar", "Februar", "Marts",
    "April", "Maj", "Juni", "Juli",
    "August", "September", "Oktober",
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
<nav>
  <ul>
    <li>Forside</li>
    <li>Skateskole</li>
    <li>Om Sesh</li>
    <li>Tilmelding</li>
    <li>Events</li>
  </ul>
</nav>
<div class="darkframe">

  

  <div class="event_calender_list">
    <div class="list_head">
       <div>
         <button id="arrow-left"></button>
         <button id="arrow-right"></button>
      </div>
      <button @click="showForm = true" class="btn_addevent">Opret Event</button>
    </div>
    <div class="month_layout">
    <h2 class="h2month">April 2023</h2>
    <hr class="line">
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
            <p class="event_ex">{{ getPlainText(event.excerpt.rendered) }}</p>
          </div>
          <div class="event_img">
            <img class="img_list" :src="getFeaturedImageUrl(event)" :alt="event.title.rendered" />
          </div>
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

  <div>
    <SeshFooter/>
  </div>
</template>

<style scoped>




/* nav*/

nav {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 2em;
}
ul {
  display: flex;
  justify-content: space-around;
  align-items: center;
  list-style: none;
  padding: 0;
  margin: 0;
  
}

li {
  margin: 0 1em;
  
}


/* layout*/
.darkframe {
  max-width: fit-content;
  background-color: rgb(0, 0, 0);
}






/* liste calender*/

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
  flex: 0 1 auto;
}

.img_list {
  max-width: 100%;
  height: auto;
}



.list_head {
  display: flex;
  justify-content: space-between;  
}

.month_layout {
  display: flex;
  align-items: center;
}

.h2month {
  margin-right: 10px;
}

.line {
  flex-grow: 1;
  height: 1px;
  background-color: black;
  margin-left: 10px;
}

/* styling */
h3 {
  color: black;
  font-size: 1.5em;
  font-weight: 600;
}

.date_style {
  justify-content: flex-start;
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

/* buttons */

#arrow-left {
  background-image: url('assets/arrowleft.svg');
  background-color: black;
  background-size: contain;
  background-repeat: no-repeat;
  padding: 1em;
}

#arrow-right {
  background-image: url('assets/arrowright.svg');
  background-color: rgb(255, 255, 255);
  background-size: contain;
  background-repeat: no-repeat;
  padding: 1em;
}
.btn_addevent {
  height: 35px;
  width: 100px; 
  background-color: #CC3333;
  color: rgb(255, 255, 255);
  
}
</style>