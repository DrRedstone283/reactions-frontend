<script setup lang="ts">
import { onMounted, reactive, toRaw } from 'vue';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import ReactionListing from './ReactionListing.vue';
const client = useSanctumClient();
const route = useRoute();

const props = defineProps({
    limit: Number,
    reactions: {
        type: Array,
        default: []
    },
    user: Object,
    reloadData: Function
})

const state = reactive({
    reactions: {},
});

const handleSelect = async function(emoji) {
    const newReaction = {
        emoji: emoji,
        info_id: route.params.id
    }

    try {
        const response = await client('/api/reactions', {
            method: "POST",
            body: newReaction
        })

        console.log(response);

        const resNewReact = await client('/api/reactions/infos/' + route.params.id);
        console.log(resNewReact);

        state.reactions = {};

        for(var i = 0; i < resNewReact.length; i++) {
            if(Object.keys(state.reactions).includes(resNewReact[i].emoji)) {
                state.reactions[resNewReact[i].emoji].push(resNewReact[i]);
            }else{
                state.reactions[resNewReact[i].emoji] = [];
                state.reactions[resNewReact[i].emoji].push(resNewReact[i]);
            }
        }
    } catch (error) {
        console.log(error);
    }
}

onMounted(() => {
    const propsReaction = toRaw(props.reactions);

    for(var i = 0; i < propsReaction.length; i++) {
        if(Object.keys(state.reactions).includes(propsReaction[i].emoji)) {
            state.reactions[propsReaction[i].emoji].push(propsReaction[i]);
        }else{
            state.reactions[propsReaction[i].emoji] = [];
            state.reactions[propsReaction[i].emoji].push(propsReaction[i]);
        }
    }
})
</script>

<template>
    <section>
        <div class="container-xl lg:container m-auto">
            <!-- Emoji Listing -->
            <div class="grid grid-cols-1 gap-2">
                <NuxtEmoji @on-select="handleSelect" class="max-w-32">
                    <template v-slot:button>
                        <button class="bg-lime-600 text-white rounded-xl py-2 px-4">Add Reaction</button>
                    </template>
                </NuxtEmoji>
                <div class="grid grid-cols-[repeat(auto-fit,_minmax(64px,65px))]">
                    <ReactionListing v-if="Object.keys(state.reactions).length > 0" v-for="reaction in state.reactions" :reaction="toRaw(reaction)" :user="props.user" :reloadData="props.reloadData"/>
                </div>
            </div>
        </div>
    </section>
</template>