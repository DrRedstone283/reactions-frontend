<script setup>
import { onBeforeUpdate, onMounted, onUpdated, reactive } from 'vue';
import PollOption from './PollOption.vue';

const props = defineProps({
    poll: Object,
    user: Object,
    reloadData: Function
})

const state = reactive({
    totalSubmits: 0
})

const test = function() {
    console.log("TEST");
}

onMounted(() => {
    for(var i = 0; i < props.poll.options.length; i++) {
        state.totalSubmits += props.poll.options[i].submits.length;
    }
})
</script>

<template>
    <div class="font-bold mb-4">
        {{ props.poll.topic }}
    </div>
    <div>
        <PollOption v-for="option in props.poll.options" :option="option" :totalSubmits="state.totalSubmits" :user="props.user" :reloadData="props.reloadData"></PollOption>
    </div>
</template>