<script setup>
const {login} = useSanctumAuth();

const form = reactive({
    email: '',
    password: ''
})

const handleLogin = async function() {
    const userCredentials = {
        email: form.email,
        password: form.password,
        remember: true
    }

    /*const userCredentials = {
        email: 'test@example.com',
        password: 'password',
    };*/

    await login(userCredentials);
}
</script>

<template>
    <section class="bg-green-50">
      <div class="container m-auto max-w-2xl py-24">
        <div class="bg-white px-6 py-8 mb-4 shadow-md rounded-md border m-4 md:m-0">
          <form @submit.prevent="handleLogin">
            <h2 class="text-3xl text-center font-semibold mb-6">Login</h2>

            <div class="mb-4">
              <label class="block text-gray-700 font-bold mb-2">E-Mail</label>
              <input
                v-model="form.email"
                type="text"
                id="name"
                name="name"
                class="border rounded w-full py-2 px-3 mb-2"
                placeholder="E-Mail"
                required
              />
            </div>
            <div class="mb-4">
              <label
                for="description"
                class="block text-gray-700 font-bold mb-2">Password</label>
              <input
                v-model="form.password"
                type="password"
                id="name"
                name="name"
                class="border rounded w-full py-2 px-3 mb-2"
                placeholder="Password"
                required
              />
            </div>

            <div>
              <button
                class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline"
                type="submit"
              >
                Login
              </button>
            </div>
          </form>
        </div>
      </div>
    </section>
</template>