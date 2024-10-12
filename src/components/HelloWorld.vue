<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "App",

  data() {
    return {
      showModal: false,
      newTudu: [],
      text: "" as string,
      text1: "" as string,
      activeIndex: null as number | null,
    };
  },

  mounted() {
    this.todos = JSON.parse(localStorage.getItem("todos")) || [];

    this.fetchJokes();
  },

  methods: {

    submit() {
      if (this.text !== "" && this.text1 !== "") {

        const tuduList = {
          text: this.text,
          text1: this.text1,
        };

        this.newTudu.push(tuduList);

        localStorage.setItem("todos", JSON.stringify(this.newTudu));

        this.text = "";
        this.text1 = "";

        this.closeModal();
      }
    },

    AddTodo() {
      this.showModal = true;
    },

    closeModal() {
      this.showModal = false;
    },

    clear(index: number) {
      this.newTudu.splice(index, 1);
    },

    toggleActive(index: number) {
      this.activeIndex = this.activeIndex === index ? null : index;
    },

    async fetchJokes() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users?_limit=5');
        const data = await response.json();
        const dataApi = data.map((item: any) => ({
          id: item.id,
          text: item.name,
          text1: item.email,
        }));

        this.newTudu = dataApi;

      } catch (error) {
        console.error(error);
      }
    }

  },
});
</script>

<template>
  <div class="container mx-auto p-6">
    <h1
      @click="AddTodo"
      class="cursor-pointer text-blue-600 font-semibold text-xl h1class text-center"
    >
      Add Todo
    </h1>
    <div
      v-if="showModal"
      class="fixed z-10 inset-0 flex items-center justify-center bg-black bg-opacity-50"
    >
      <div class="bg-white p-6 rounded shadow-lg">
        <form @submit.prevent="submit">
          <h1>Title</h1>
          <input
            type="text"
            v-model="text"
            placeholder="Yangi tudu kiriting"
            class="border p-2 mb-4 w-full rounded"
          />
          <h1>Description</h1>
          <input
            type="text"
            v-model="text1"
            placeholder="Yangi tudu kiriting"
            class="border p-2 mb-4 w-full rounded"
          />
          <div class="flex justify-end">
            <button
              type="submit"
              class="bg-blue-500 text-white px-4 py-2 rounded"
            >
              Add
            </button>
            <button
              type="button"
              @click="closeModal"
              class="bg-gray-300 text-gray-700 px-4 py-2 rounded ml-2"
            >
              Close
            </button>
          </div>
        </form>
      </div>
    </div>
    <ol>
      <li
        v-for="(item, index) in newTudu"
        :key="index"
        :class="{ active: activeIndex === index }"
        class="border rounded-lg p-2 mt-3"
      >
        <span>
          <p class="text-2xl font-serif">{{ item.text }}</p>
          <p class="">{{ item.text1 }}</p>
        </span>
        <span class="flex justify-center items-center">
          <input class="mr-2" type="checkbox" @click="toggleActive(index)" />
          <button
            @click="clear(index)"
            icon="x-circle"
            scale="2"
            variant="danger"
          >
            <svg
              data-v-ce1a3ef4
              width="22"
              height="22"
              viewBox="0 0 22 22"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M1 5H21M9 10V15M13 10V15M3 5H19L17.42 19.22C17.3658 19.7094 17.1331 20.1616 16.7663 20.49C16.3994 20.8184 15.9244 21 15.432 21H6.568C6.07564 21 5.60056 20.8184 5.23375 20.49C4.86693 20.1616 4.63416 19.7094 4.58 19.22L3 5ZM6.345 2.147C6.50675 1.80397 6.76271 1.514 7.083 1.31091C7.4033 1.10782 7.77474 0.999996 8.154 1H13.846C14.2254 0.999806 14.5971 1.10755 14.9176 1.31064C15.2381 1.51374 15.4942 1.80381 15.656 2.147L17 5H5L6.345 2.147V2.147Z"
                stroke="#FF4545"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              ></path>
            </svg>
          </button>
        </span>
      </li>
    </ol>
  </div>
</template>

<style scoped>
.active {
  text-decoration: line-through;
}
</style>
