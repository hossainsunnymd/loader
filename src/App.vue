<script setup>
import { ref } from "vue";
const products = ref([]);
const loader = ref(true);
callApi();
async function callApi() {
  const res = await fetch("https://dummyjson.com/products");
  const json = await res.json();
  await nonBlockingLoop(300);
  loader.value = false;
  products.value = json["products"];
}
function nonBlockingLoop(count) {
  return new Promise((resolve) => {
    let i = 0;
    function loop() {
      if (i <= count) {
        console.log(i);
        i++;
        if (i == count) {
          resolve();
          return;
        } else {
          setTimeout(loop, 0); 
        }
      }
    }
    loop();
  });
}

</script>

<template>
  <div class="table-container">
    <div v-if="loader" class="loader-center">
      <div class="loader"></div>
    </div>

    <div class="overflow-x-auto shadow-md sm:rounded-lg">
      <table
        class="w-[1300px] mx-auto text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400"
      >
        <thead
          class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400"
        >
          <tr>
            <th scope="col" class="px-6 py-3">Product name</th>
            <th scope="col" class="px-6 py-3">Title</th>
            <th scope="col" class="px-6 py-3">Description</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(item, index) in products"
            :key="index"
            class="odd:bg-white odd:dark:bg-gray-900 even:bg-gray-50 even:dark:bg-gray-800 border-b dark:border-gray-700"
          >
            <td class="px-6 py-4">
              {{ item.id }}
            </td>
            <td class="px-6 py-4">
              {{ item.title }}
            </td>
            <td class="px-6 py-4">
              {{ item.description }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.table-container {
  position: relative;
  width: 100%;
  height: 100vh;
}

.loader-center {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
}

.loader {
  width: 100px;
  height: 40px;
  --g: radial-gradient(
      farthest-side,
      #0000 calc(95% - 3px),
      black calc(100% - 3px) 98%,
      #0000 101%
    )
    no-repeat;
  background: var(--g), var(--g), var(--g);
  background-size: 30px 30px;
  animation: l9 1s infinite alternate;
}

@keyframes l9 {
  0% {
    background-position: 0 50%, 50% 50%, 100% 50%;
  }
  20% {
    background-position: 0 0, 50% 50%, 100% 50%;
  }
  40% {
    background-position: 0 100%, 50% 0, 100% 50%;
  }
  60% {
    background-position: 0 50%, 50% 100%, 100% 0;
  }
  80% {
    background-position: 0 50%, 50% 50%, 100% 100%;
  }
  100% {
    background-position: 0 50%, 50% 50%, 100% 50%;
  }
}
</style>
