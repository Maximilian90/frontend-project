<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  customFieldValue: {
    type: String,
    required: true,
    default: '',
  },
});
</script>

<template>
  <p>{{ customFieldValue }}</p>
</template>