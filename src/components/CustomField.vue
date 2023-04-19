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
  <div class="custom-field-block">
    <p>{{ customFieldValue }}</p>
  </div>
</template>