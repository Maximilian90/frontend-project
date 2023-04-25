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
    title:      title.value,
    date:       date.value,
    location:   location.value,
    content:    content.value,

  };

const encodedUser = btoa(`${username.value}:${password.value}`);

  // Send the POST request to the server
  fetch('https://sesh.mg-visions.com/index.php/wp-json/wp/v2/posts', {
    // The method have to be the type of post, so the server knows we create a new post
    method: 'post',
    // Headers sent along the post
    headers: {
      // Tells the server, that it can expect JSON
      'Content-Type':  'application/json',
      // Send the username and password to the server
      'Authorization': `Basic ${encodedUser}`
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

    <form @submit.prevent="onCreatePost">

        <div>
            <label for="title">Title</label>
            <div>
                <input
                type="text"
                id="title"
                v-model="title"
                >
            </div>
        </div>

        <div>
            <label for="date">Dato</label>
            <div>
                <textarea
                v-model="date"
                id="date"
                ></textarea>
            </div>
        </div>

        <div>
            <label for="location">Adresse</label>
            <div>
                <textarea
                v-model="location"
                id="location"
                ></textarea>
            </div>
        </div>

        <div>
            <label for="content">Content</label>
            <div>
                <textarea
                v-model="content"
                id="content"
                ></textarea>
            </div>
        </div>

    <button type="submit">Create Post</button>

</form>

</template>

<style scoped>
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 9999;
}
</style>