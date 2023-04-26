<script setup>
import { ref } from 'vue';
import { defineProps } from 'vue';

const props = defineProps({
  title: String,
});

const title = ref('');
const date = ref('');
const location = ref('');
const content = ref('');
const username = ref('');
const password = ref('');

const onCreatePost = () => {
  // The fields send, to create post
  const body = {
    title: title.value,
    date: date.value,
    location: location.value,
    content: content.value,

  };

  const encodedUser = btoa(`${username.value}:${password.value}`);

  // Send the POST request to the server
  fetch('https://sesh.mg-visions.com/index.php/wp-json/wp/v2/event', {
    // The method have to be the type of post, so the server knows we create a new post
    method: 'post',
    // Headers sent along the post
    headers: {
      // Tells the server, that it can expect JSON
      'Content-Type': 'application/json',
      // Send the username and password to the server
      'Authorization': `Basic YWRtaW46YWRtaW4=`
    },
    // This is the body of the server request e.i. the fields sent along
    body: JSON.stringify(body),
  })
    // Success response
    .then((response) => {
      console.log(response);
    })
    // Error response
    .catch((error) => {
      console.log(error);
    });
};

</script>

<template>
  <div class="form-overlay">
    <div class="form_container">

      <form @submit.prevent="onCreatePost">
        <h2 class="formtitel" >Opret Event</h2>
        <p class="formbeskrivelse">Få dit skaterevent vist på vores eventside ved at udfylde alle nedenstående felter</p>
        <div>
          <label for="title">Event Navn</label>
          <div>
            <input type="text" id="title" v-model="title">
          </div>
        </div>

        <div>
          <label for="date">Dato</label>
          <div>
            <textarea v-model="date" id="date"></textarea>
          </div>
        </div>

        <div>
          <label for="location">Adresse</label>
          <div>
            <textarea v-model="location" id="location"></textarea>
          </div>
        </div>

        <div>
          <label for="content">Beskrivelse</label>
          <div>
            <textarea v-model="content" id="content"></textarea>
          </div>
        </div>

        <button class="createpostknap" type="submit">Opret Event</button>

      </form>
    </div>
  </div>
</template>

<style scoped>
.form-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

/* Center the form contents vertically and horizontally */
.form-overlay form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  max-width: 500px;
  background-color: #0a0a0a;
  padding: 3rem;
  border-radius: 5px;
}

/* Style form input fields and buttons */
.form-overlay input,
.form-overlay button {
  width: 100%;
  padding: 1rem;
  margin-bottom: 1rem;
  border-radius: 5px;
}

.form-overlay button {
  background-color: #CC3333;
  color: #ffffff;
  font-weight: bold;
  border: none;
  cursor: pointer;
  width: 130px;
}

.form-overlay button:hover {
  background-color: #CC3333;
}

#title {
  width: 300px;
  height: 0px;
}

#date, #location {
  width: 300px;
  height: 30px;
  margin-bottom: 10px;
  border-radius: 5px;
}

#content {
  width: 300px;
  height: 100px;
  margin-bottom: 30px;
  border-radius: 5px;
}

.formtitel {
  justify-content: flex-start;
  margin-bottom: 10px;
  margin-top: 10px;
  font-weight: 500;
}

.formbeskrivelse {
  margin-bottom: 20px;
  width: auto;
  font-size: 14px;
}

</style>