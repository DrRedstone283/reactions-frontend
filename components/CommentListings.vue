<script setup lang="ts">
import { onMounted, reactive } from 'vue';
import CommentListing from './CommentListing.vue';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
const client = useSanctumClient();
const route = useRoute();

const props = defineProps({
    limit: Number,
    showButton: {
        type: Boolean,
        default: false
    },
    comments: {
        type: Array,
        default: []
    },
    user: Object,
    reloadData: Function
})

onMounted(async () => {
    try {
        
    } catch (error) {
        console.error(error);
    }
})
</script>

<template>
    <section class="py-5">
        <div class="container-xl lg:container m-auto">
            <!-- Comment Listing -->
            <div class="grid grid-cols-1 gap-3">
                <CommentListing v-if="props.comments.length > 0" v-for="comment in props.comments.slice(0, limit || props.comments.length)" :key="comment.id" :comment="comment" :user="props.user" :reloadData="props.reloadData"/>
                <div v-else class="text-gray-400">No Comments</div>
            </div>
        </div>
    </section>
</template>