<script setup>

const props = defineProps({
    info: Object
});

const showFullDescription = ref(false);

const toggleFullDescription = function() {
    showFullDescription.value = !showFullDescription.value;
}

const truncatedDescription = computed(function() {
    let description = props.info.description;
    if(!showFullDescription.value) {
        description = description.substring(0, 400) + '...';
    }
    return description;
})
</script>

<template>
    <div class="bg-white rounded-xl shadow-md relative">
        <div class="p-4">
            <div class="mb-6">
                <h3 class="text-xl font-bold">{{ info.title }}</h3>
            </div>

            <div class="mb-5">
                <div v-if="props.info.description.length >= 400">
                    <div>
                        {{ truncatedDescription }}
                    </div>
                    <button @click="toggleFullDescription" class="text-green-500 hover:text-green-600 mb-5">
                        {{ showFullDescription ? 'Less' : 'More' }}
                    </button>
                </div>
                <div v-else>
                    {{ props.info.description }}
                </div>
            </div>

            <div class="border border-gray-100 mb-5"></div>

            <div class="flex flex-col lg:flex-row justify-between mb-4">
                <RouterLink
                  :to="'/infos/' + info.id"
                  class="h-[36px] bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg text-center text-sm"
                >
                  Read More
                </RouterLink>
            </div>
        </div>
    </div>
</template>