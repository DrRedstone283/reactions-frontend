<script setup>
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import BackButton from '/components/BackButton.vue';
import { onBeforeUpdate, onUpdated, reactive } from 'vue';
import ReactionListings from '../../components/ReactionListings.vue';
const client = useSanctumClient();

const route = useRoute();
const router = useRouter();

const form = reactive({
  message: ''
})

const handleSubmit = async function() {
  const newComment = {
    message: form.message,
    info_id: state.info.id,
  }

  try {
    const response = await client('/api/comments', {
      method: "POST",
      body: newComment
    })
    
    console.log(response);

    const resNewCom = await client('/api/comments/infos/' + state.info.id);
    console.log(resNewCom);

    state.info.comments = resNewCom;

    form.message = '';
  } catch (error) {
    console.log(error)
  }
}

const state = reactive({
  info: {},
  user: {},
  isLoading: true
});

const deleteInfo = async function() {
  try {
    const confirm = window.confirm('Are you sure you want to delete this info?');
    if(confirm) {
      const response = await client('/api/infos/' + route.params.id, {
        method: "DELETE"
      });

      console.log(response);
      //router.push('/infos');
    }
  }catch (error) {
    console.error('Error deleting job', error);
  }
}

const reloadData = async function() {
  try {
    const resInf = await client('/api/infos/' + route.params.id);
    console.log(resInf);
    state.info = resInf;
    
    const resUser = await client('/api/user');
    state.user = resUser;
  }catch (error) {
    console.error('Error fetching job', error);
  }
}

onMounted(async function() {
  try {
    const resInf = await client('/api/infos/' + route.params.id);
    console.log(resInf);
    state.info = resInf;
    
    const resUser = await client('/api/user');
    state.user = resUser;
  }catch (error) {
    console.error('Error fetching job', error);
  }finally {
    state.isLoading = false;
  }
})
</script>

<template>
    <BackButton></BackButton>
      <section v-if="!state.isLoading" class="bg-green-50">
        <div class="container m-auto py-10 px-6">
          <div class="grid grid-cols-1 md:grid-cols-70/30 w-full gap-6">
            <main>
              <div class="bg-white p-6 rounded-lg shadow-md">
                <h1 class="text-3xl font-bold mb-4">{{state.info.title}}</h1>
  
                <p class="mb-4">
                  {{state.info.description}}
                </p>


                <!-- Poll -->
                <div v-if="state.info.poll">
                  <hr class="my-4" />
                  <Poll :key="state.info.poll" :poll="state.info.poll" :user="state.user" :reloadData="reloadData"></Poll>
                </div>

                <hr class="my-4" />

                <!-- Reactions -->
                <ReactionListings v-if="state.info.state_react" :key="state.info.reactions.length" :reactions="state.info.reactions" :user="state.user" :reloadData="reloadData"></ReactionListings>
                <div v-else class="text-gray-400 mt-5">
                  Reactions are disabled
                </div>
              </div>
            </main>
  
            <!-- Sidebar -->
            <aside>
              <!-- Comments -->
              <div class="bg-white p-6 rounded-lg shadow-md">
                <h3 class="text-xl font-bold">Comments</h3>
  

                <div v-if="state.info.state_com">
                  <CommentListings :key="state.info.comments.length" :comments="state.info.comments" :showButton="false" :user="state.user" :reloadData="reloadData"></CommentListings>

                  <hr class="my-4" />

                  <form @submit.prevent="handleSubmit">
                    <h3 class="text-l">Write Comment:</h3>
                    <textarea v-model="form.message" id="message" name="message" rows="2" class="my-2 bg-green-100 p-2 w-full" placeholder="Message" required></textarea>
                    <button type="submit"
                    class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded block">Send</button>
                  </form>
                </div>
                <div v-else class="text-gray-400 mt-5">
                  Comments are disabled
                </div>
              </div>
  
              <!-- Manage -->
              <div class="bg-white p-6 rounded-lg shadow-md mt-6">
                <h3 class="text-xl font-bold mb-6">Manage Info</h3>
                <RouterLink
                  :to="`/infos/edit/${state.info.id}`"
                  class="bg-green-500 hover:bg-green-600 text-white text-center font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block"
                  >Edit Info</RouterLink
                >
                <button @click="deleteInfo"
                  class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block"
                >
                  Delete Info
                </button>
              </div>
            </aside>
          </div>
        </div>
      </section>
  
      <div v-else class="text-center text-gray-500 py-6">
        <PulseLoader></PulseLoader>
      </div>
  </template>