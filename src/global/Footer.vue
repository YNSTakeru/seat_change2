<template>
  <footer class="w-screen">
    <div
      class="
        fixed
        h-16
        w-full
        bg-red-400
        text-lg
        p-2
        px-4
        bottom-0
        left-0
        flex
        justify-between
        items-center
      "
    >
      <button
        class="rounded-full p-2 px-4 bg-indigo-600 text-white"
        @click="changeSeat"
      >
        席替え
      </button>
      <button
        class="rounded-full p-2 px-4 border border-black text-black"
        @click="openConfig"
      >
        設定
      </button>
      <div
        id="config"
        class="
          hidden
          absolute
          w-full
          h-60
          bg-white
          bottom-0
          left-0
          text-1xl text-black
          flex
          justify-center
          items-center
          flex-col
          gap-8
        "
      >
        <div
          class="
            absolute
            top-2
            right-2
            w-10
            h-auto
            border border-red-400
            rounded-full
          "
          @click="closeConfig"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="red"
            class="bi bi-x"
            viewBox="0 0 16 16"
          >
            <path
              d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z"
            />
          </svg>
        </div>
        <label for="cols"
          >列数<input
            class="ml-6 border border-black w-10 text-center p-1"
            type="number"
            id="cols"
            v-model="getCols"
            min="1"
        /></label>
        <label for="cols"
          >席数<input
            class="ml-6 border border-black w-10 text-center p-1"
            type="number"
            id="cols"
            v-model.lazy="getSeats"
            min="1"
        /></label>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  data() {
    return {
      cols: 4,
      seats: 24,
    };
  },
  computed: {
    getCols: {
      get() {
        return this.cols;
      },
      set(value) {
        this.cols = value;
        this.$emit("open-config", { cols: value, seats: this.seats });
      },
    },
    getSeats: {
      get() {
        return this.seats;
      },
      set(value) {
        this.seats = value;
        this.$emit("open-config", { cols: this.cols, seats: value });
      },
    },
  },
  methods: {
    changeSeat() {
      this.$emit("change-seat");
    },
    openConfig() {
      const config = document.getElementById("config");
      config.classList.toggle("hidden");
    },
    closeConfig() {
      const config = document.getElementById("config");
      config.classList.toggle("hidden");
    },
  },
};
</script>

<style>
</style>