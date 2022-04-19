<template>
  <div>
    <div class="flex items-center justify-center ">
      <div class="px-4 lg:py-12">
        <div class="lg:gap-4 lg:flex">
          <div
            class="flex flex-col items-center justify-center md:py-24 lg:py-32"
          >
            <h1 class="font-bold text-blue-600 text-9xl">404</h1>
            <p
              class="
                mb-2
                text-2xl
                font-bold
                text-center text-gray-800
                md:text-3xl
              "
            >
              <span class="text-red-500">Oops!</span> Page not found
            </p>
            <p class="mb-8 text-center text-gray-500 md:text-lg">
              The page you’re looking for doesn’t exist.
            </p>
            <n-link
              class="px-6 py-2 text-sm font-semibold text-blue-800 bg-blue-100"
              :to="'/'"
            >
            Go Home
            </n-link>
          </div>
          <div class="mt-4">
            <img
              :src="require(`~/assets/images/error.png`)"
              alt="img"
              class="object-cover w-full h-full"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>