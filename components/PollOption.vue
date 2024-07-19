<script setup>
import { onBeforeMount, onMounted, reactive } from 'vue';

const client = useSanctumClient();
const propsOpt = defineProps({
    option: Object,
    user: Object,
    totalSubmits: Number,
    reloadData: Function
})

const state = reactive({
    option: {},
    userId: '',
    userVoted: false
})

const handleSubmit = async function() {
    try {
        const response = await client('/api/polls/' + propsOpt.option.poll_id + '/options/' + propsOpt.option.id + '/submits', {
            method: "POST"
        })

        console.log(response);
        state.userId = response.user_id;

        const resNewOpt = await client('/api/polls/' + propsOpt.option.poll_id + '/options/' + propsOpt.option.id);

        state.option = resNewOpt;

        state.userVoted = false;
        for(var i = 0; i < state.option.submits.length; i++) {
            if(state.option.submits[i].user.id == state.userId) {
                state.userVoted = true;
            }
        }

        propsOpt.reloadData();
    } catch (error) {
        console.log(error);
    }
}

onBeforeMount(async () => {
    state.option = propsOpt.option;

    for(var i = 0; i < state.option.submits.length; i++) {
        if(state.option.submits[i].user.id == propsOpt.user.id) {
            state.userVoted = true;
        }
    }
})
</script>

<template>
    <button @click="handleSubmit" class="mb-2 bg-gray-200 w-full hover:bg-gray-300">
        <div v-if="state.userVoted" class="grid grid-cols-[3fr_120px] p-4 w-full h-full bg-green-300">
            <div class="text-left">
                {{ state.option.title }}
            </div>
            <v-tooltip v-if="state.option.submits.length > 0">
                <span v-for="submit in state.option.submits">{{ submit.user.name }}<br></span>
                <template v-slot:activator="{ props }">
                    <label v-bind="props" class="text-right">
                        {{ state.option.submits.length }} Stimmen
                    </label>
                </template>
            </v-tooltip>

            <label v-else class="text-right">
                0 Stimmen
            </label>
        </div>
        <div v-else class="grid grid-cols-[3fr_100px] p-4">
            <div class="text-left">
                {{ state.option.title }}
            </div>
            <v-tooltip v-if="state.option.submits.length > 0">
                <span v-for="submit in state.option.submits">{{ submit.user.name }}<br></span>
                <template v-slot:activator="{ props }">
                    <label v-bind="props" class="text-right">
                        {{ state.option.submits.length }} Stimmen
                    </label>
                </template>
            </v-tooltip>

            <label v-else class="text-right">
                0 Stimmen
            </label>
        </div>
    </button>
</template>