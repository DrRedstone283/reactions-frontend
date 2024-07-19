<script setup>
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import { onBeforeMount, onMounted, onUpdated, reactive } from 'vue';
const client = useSanctumClient();


const propsReact = defineProps({
    reaction: Array,
    user: Object,
    reloadData: Function
});

const state = reactive({
    byUser: false
})

const handleDeletion = async function() {
    try {
        for(var i = 0; i < propsReact.reaction.length; i++) {
            if(propsReact.reaction[i].user_id == propsReact.user.id) {
                await client("/api/reactions/" + propsReact.reaction[i].id, {
                    method: "DELETE"
                });
            }
        }

        propsReact.reloadData();
    } catch (error) {
        console.error(error);
    }
}

const updateDeletionState = function() {
    for(var i = 0; i < propsReact.reaction.length; i++) {
        if(propsReact.reaction[i].user_id == propsReact.user.id) {
            state.byUser = true;
        }
    }
}

onMounted(() => {
    updateDeletionState();
})

onUpdated(() => {
    updateDeletionState();
})
</script>

<template>
    <div class=" relative">
        <div class="p-2 w-16 h-14">
            <v-badge v-if="propsReact.reaction.length > 1" :content="propsReact.reaction.length">
                <v-tooltip>
                    <span v-for="reaction in propsReact.reaction">{{ reaction.user.name }}<br></span>
                    <template v-slot:activator="{ props }">
                        <button @click.prevent="handleDeletion" v-if="state.byUser" class="bg-red-500 text-white font-bold px-2 rounded-full absolute left-0 bottom-7">
                            <p class="text-sm">x</p>
                        </button>
                        <div v-bind="props" class="bg-green-100 w-min rounded-full p-3">{{ propsReact.reaction[0].emoji }}</div>
                    </template>
                </v-tooltip>
            </v-badge>

            <v-tooltip v-else>
                <span>{{propsReact.reaction[0].user.name}}</span>
                <template v-slot:activator="{ props }">
                    <button @click.prevent="handleDeletion" v-if="state.byUser" class="bg-red-500 text-white font-bold px-2 rounded-full absolute right-1 top-0">
                        <p class="text-sm">x</p>
                    </button>
                    <div v-bind="props" class="bg-green-100 w-min rounded-full p-3 pt-2 h-full">{{ propsReact.reaction[0].emoji }}</div>
                </template>
            </v-tooltip>
        </div>
    </div>
</template>