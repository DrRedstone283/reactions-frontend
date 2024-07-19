<script setup>
import axios from 'axios';
import { onBeforeMount } from 'vue';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
const client = useSanctumClient();

defineProps({
    limit: Number,
    showButton: {
        type: Boolean,
        default: false
    }
})

const state = reactive({
    infos: [],
    isLoading: true
})

onMounted(async function() {
    try {
        const response = await client('/api/infos');
        console.log(response);
        state.infos = response;
    } catch (error) {
        console.error(error);
    } finally {
        state.isLoading = false;
    }
})
</script>

<template>
    <section class="bg-blue-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">Browse Infos</h2>
            
            <!-- Loading Spinner -->
            <div v-if="state.isLoading" class="text-center text-gray-500 py-6">
                <PulseLoader></PulseLoader>
            </div>

            <!-- Info Listing -->
            <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <InfoListing v-for="info in state.infos.slice(0, limit || state.infos.length)" :key="info.id" :info="info"/>
            </div>
        </div>
    </section>

    <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
      <RouterLink
        to="/infos"
        class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
        >View All Infos</RouterLink>
    </section>
</template>