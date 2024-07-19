<template>
    <section class="py-4">
      <div class="container-xl lg:container m-auto">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 p-4 rounded-lg">
          <Card>
            <h2 class="text-2xl font-bold">For Employees</h2>
            <p class="mt-2 mb-4">
              Browse our infos and stay up to date
            </p>
            <RouterLink
              to="/infos"
              class="inline-block bg-black text-white rounded-lg px-4 py-2 hover:bg-gray-700"
            >
              Browse Infos
            </RouterLink>
          </Card>
          <Card bg="bg-green-100">
            <h2 class="text-2xl font-bold">For Employers</h2>
            <p class="mt-2 mb-4">
              List your info to keep your employees informed
            </p>
            <RouterLink
              to="/infos/add"
              class="inline-block bg-green-500 text-white rounded-lg px-4 py-2 hover:bg-green-600"
            >
              Add Info
            </RouterLink>
          </Card>
        </div>
      </div>
    </section>
</template>