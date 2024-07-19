<script setup>
const client = useSanctumClient();

const router = useRouter();

const form = reactive({
    title: '',
    description: '',
    reactions: 'Disabled',
    comments: 'Disabled',
    poll_status: 'false',
    poll_multi_select: 'false',
    poll_topic: '',
    poll_option_1: '',
    poll_option_2: '',
    poll_option_3: '',
    poll_option_4: ''
})

const handleSubmit = async function() {
  const newInfo = {
    title: form.title,
    description: form.description,
    reactions: false,
    comments: false
  }

  if(form.reactions == 'Enabled') newInfo.reactions = true;
  if(form.comments == 'Enabled') newInfo.comments = true;

  try {
    const resInfo = await client('/api/infos', {
      method: "POST",
      body: newInfo
    })

    if(form.poll_status == true) {
      const newPoll = {
        topic: form.poll_topic,
        multi_select: form.poll_multi_select,
        info_id: resInfo.id
      }

      console.log(newPoll);

      const resPoll = await client('/api/polls', {
        method: "POST",
        body: newPoll
      })

      console.log(resInfo.id);
      console.log(resPoll);

      const newPollOption1 = {
        title: form.poll_option_1,
        poll_id: resPoll.id
      }

      const newPollOption2 = {
        title: form.poll_option_2,
        poll_id: resPoll.id
      }

      const resPollOpt1 = await client('/api/polls/' + resPoll.id + '/options', {
        method: "POST",
        body: newPollOption1
      })

      const resPollOpt2 = await client('/api/polls/' + resPoll.id + '/options', {
        method: "POST",
        body: newPollOption2
      })

      console.log(resPollOpt1);
      console.log(resPollOpt2);

      if(form.poll_option_3 != '') {
        const newPollOption3 = {
          title: form.poll_option_3,
          poll_id: resPoll.id
        }

        const resPollOpt3 = await client('/api/polls/' + resPoll.id + '/options', {
          method: "POST",
          body: newPollOption3
        })
      }

      if(form.poll_option_4 != '') {
        const newPollOption4 = {
          title: form.poll_option_4,
          poll_id: resPoll.id
        }

        const resPollOpt4 = await client('/api/polls/' + resPoll.id + '/options', {
          method: "POST",
          body: newPollOption4
        })
      }
    }

    router.push(`/infos/${resInfo.id}`);
  } catch (error) {
    console.error(error);
  }
}
</script>

<template>
    <section class="bg-green-50">
      <div class="container m-auto max-w-2xl py-24">
        <div class="bg-white px-6 py-8 mb-4 shadow-md rounded-md border m-4 md:m-0">
          <form @submit.prevent="handleSubmit">
            <h2 class="text-3xl text-center font-semibold mb-6">Add Info</h2>

            <div class="mb-4">
              <label class="block text-gray-700 font-bold mb-2">Title</label>
              <input
                v-model="form.title"
                type="text"
                id="name"
                name="name"
                class="border rounded w-full py-2 px-3 mb-2"
                placeholder="Announcements"
                required
              />
            </div>
            <div class="mb-4">
              <label
                for="description"
                class="block text-gray-700 font-bold mb-2">Description</label>
              <textarea
                v-model="form.description"
                id="description"
                name="description"
                class="border rounded w-full py-2 px-3"
                rows="4"
                placeholder="Announcements"
                required
              ></textarea>
            </div>
            <hr class="my-4" />
            <div class="mb-4">
              <input v-model="form.poll_status" id="pollStatus" name="pollStatus" type="checkbox">
              <label for="pollStatus" class="ml-2 text-gray-700 font-bold mb-2">Umfrage</label>
            </div>
            <div v-if="form.poll_status == true">
              <div class="mb-4 mt-8">
                <input v-model="form.poll_multi_select" type="checkbox">
                <label class="ml-2 text-gray-700 mb-2">Multi-Select</label>
              </div>
              <div class="mb-4">
                <label class="block text-gray-700 font-bold mb-2">Topic</label>
                <input
                  v-model="form.poll_topic"
                  type="text"
                  class="border rounded w-full py-2 px-3 mb-2"
                  placeholder="Topic"
                  required
                ></input>
              </div>
              <div class="mb-2">
                <label class="block text-gray-700 font-bold mb-2">Option 1</label>
                <input
                  v-model="form.poll_option_1"
                  type="text"
                  class="border rounded w-full py-2 px-3 mb-2"
                  placeholder="Option 1"
                  required
                ></input>
              </div>
              <div class="mb-2">
                <label class="block text-gray-700 font-bold mb-2">Option 2</label>
                <input
                  v-model="form.poll_option_2"
                  type="text"
                  class="border rounded w-full py-2 px-3 mb-2"
                  placeholder="Option 2"
                  required
                ></input>
              </div>
              <div class="mb-2">
                <label class="block text-gray-700 font-bold mb-2">Option 3</label>
                <input
                  v-model="form.poll_option_3"
                  type="text"
                  class="border rounded w-full py-2 px-3 mb-2"
                  placeholder="Option 3 (optional)"
                ></input>
              </div>
              <div class="mb-4">
                <label class="block text-gray-700 font-bold mb-2">Option 4</label>
                <input
                  v-model="form.poll_option_4"
                  type="text"
                  class="border rounded w-full py-2 px-3 mb-2"
                  placeholder="Option 4 (optional)"
                ></input>
              </div>
            </div>
            <hr class="my-4" />
            <div class="mb-4">
              <label for="reactions" class="block text-gray-700 font-bold mb-2"
                >Reactions</label
              >
              <select
                v-model="form.reactions"
                id="reactions"
                name="reactions"
                class="border rounded w-full py-2 px-3"
                required
              >
                <option value="Enabled">Enabled</option>
                <option value="Disabled">Disabled</option>
              </select>
            </div>
            <div class="mb-4">
              <label for="comments" class="block text-gray-700 font-bold mb-2"
                >Comments</label
              >
              <select
                v-model="form.comments"
                id="comments"
                name="comments"
                class="border rounded w-full py-2 px-3"
                required
              >
                <option value="Enabled">Enabled</option>
                <option value="Disabled">Disabled</option>
              </select>
            </div>

            <div>
              <button
                class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline"
                type="submit"
              >
                Add Info
              </button>
            </div>
          </form>
        </div>
      </div>
    </section>
</template>