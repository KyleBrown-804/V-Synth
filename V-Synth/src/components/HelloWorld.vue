<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { invoke } from '@tauri-apps/api';

let message = ref('default');
onMounted(() => {
  invoke('greet', { name: 'From Rust' }).then((resp) => message.value = resp as string);
});
</script>

<template>
  <h1>{{ message }}</h1>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
