<script setup>
    import { ref } from 'vue';
    const topic = ref('');
    const emit = defineEmits(['start-quiz']);
    const props = defineProps(['errorMessage']);
</script>

<template>
    <section class="start-screen container">
        <h2>Choose a topic to start testing your knowledge</h2>
        <input v-model="topic" type="text" placeholder="Enter your topic...">
        <button :disabled="!topic" @click="emit('start-quiz', topic)"  >Start Quiz</button>
        <small class="error" v-if="errorMessage">There was an error, please try again later.</small>
    </section>
</template>