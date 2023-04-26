<script setup>
import { defineComponent, provide } from 'vue'
import { createRouter, createWebHistory } from 'vue-router';
import Forside from './Forside.vue'
import About from './Event.vue'
import Eventdetail from './components/Eventdetail.vue'

const routes = [
  {
    path: '/',
    component: Forside
  },
  {
    path: '/Event',
    component: About
  },
  {
    path: '/Eventdetail',
    component: Eventdetail
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: '/', component: Forside },
    { path: '/Event', component: About },
    { path: '/Eventdetail', component: Eventdetail }
  ]
})
// Expose router to other components
provide('router', router.currentRoute)

const App = defineComponent({
  name: 'App',
  setup() {
    return {
      router
    }
  }
})
</script>

<template>
    <div>
        <router-link to="/">Home</router-link>
        <router-link to="/Event">Event</router-link>
        <router-link to="/Eventdetail">Event Detail</router-link>
  
      <router-view></router-view>
    </div>
</template>