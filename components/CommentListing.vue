<script setup>
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import { onMounted, reactive } from 'vue';
const client = useSanctumClient();
const router = useRouter();

const props = defineProps({
    comment: Object,
    user: Object,
    reloadData: Function
});

const showFullMessage = ref(false);

const toggleFullMessage = function() {
    showFullMessage.value = !showFullMessage.value;
}

const truncatedMessage = computed(function() {
    let message = props.comment.message;
    if(!showFullMessage.value) {
        message = message.substring(0, 200) + '...';
    }
    return message;
})

const handleDeletion = async function() {
    try{
        await client('/api/comments/' + props.comment.id, {
            method: "DELETE"
        });

        props.reloadData();
    }catch (error) {
        console.error(error);
    }
}
</script>

<template>
    <div class="bg-green-50 rounded-l shadow-md relative">
        <div class="p-4">
            <div class="grid grid-cols-[1fr_70px]">
                <div class="font-bold">
                    {{ props.comment.user.name }}
                </div>
                <button v-if="props.comment.user_id == props.user.id" @click.prevent="handleDeletion" class="bg-red-500 py-1 px-2 rounded-md text-white font-bold">Delete</button>
            </div>
            <hr class="my-2" />
            <div class="text-sm text-gray-400">
                {{ new Date(props.comment.created_at).toLocaleDateString('de-DE', {weekday: 'long', day: '2-digit', month: '2-digit', year: 'numeric', hour: '2-digit', minute: '2-digit'}) }}
            </div>
            <div v-if="props.comment.message.length >= 200">
                <div>
                    {{ truncatedMessage }}
                </div>
                <button @click="toggleFullMessage" class="text-green-500 hover:text-green-600 mb-5">
                    {{ showFullMessage ? 'Less' : 'More' }}
                </button>
            </div>
            <div v-else>
                {{ props.comment.message }}
            </div>
        </div>
    </div>
</template>