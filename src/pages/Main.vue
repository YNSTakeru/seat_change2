<template>
  <main class="w-screen pt-4 text-lg">
    <div class="grid grid-cols-4 gap-2 mb-8">
      <div class="border w-full h-10"></div>
      <div
        class="border w-full h-10 col-span-2 flex justify-center items-center"
      >
        <div class="border bg-white p-2">ホワイトボード</div>
      </div>
      <div class="border w-full h-10 flex justify-center items-center">
        <div class="bg-green-400 text-indigo-600 p-2 whitespace-nowrap">
          出入り口
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  props: ["changeSeatBtnClicked", "config"],
  data() {
    return {
      cols: 4,
      num: 24,
      seats: { num: 24, can: [] },
      students: [],
    };
  },
  watch: {
    changeSeatBtnClicked: {
      handler() {
        if (this.changeSeatBtnClicked) {
          this.$emit("reset-btn-flag");
          this.changeSeat();
        }
      },
      deep: true,
    },
    config: {
      handler() {
        const configSeats = parseInt(this.config.seats);

        this.cols = this.config.cols;
        this.seats.can = [];
        for (let i = 0; i < configSeats; i++) {
          this.seats.can.push(true);
        }
        const seats = document.querySelector("#seats");
        seats.setAttribute("class", `grid grid-cols-${this.cols} gap-4`);
        if (this.seats.num < configSeats) {
          let num = configSeats - this.seats.num;
          for (let i = 0; i < num; i++) {
            const gridItems = document.createElement("div");
            gridItems.setAttribute(
              "class",
              `border w-full h-10 bg-red-400 flex justify-center items-center`
            );

            const nameTextContainer = document.createElement("div");
            const nameText = document.createElement("input");
            nameText.setAttribute(
              "class",
              `w-full text-center bg-transparent border-opacity-0" type="text`
            );
            nameText.setAttribute("value", `追加${i}`);
            nameTextContainer.appendChild(nameText);
            nameTextContainer.setAttribute(
              "class",
              `white-space-nowrap p-2 text-indigo-700`
            );

            gridItems.addEventListener("click", () => {
              this.seats.can[i] = !this.seats.can[i];
              gridItems.classList.toggle("bg-red-400");
              gridItems.classList.toggle("bg-blue-400");
            });

            nameText.addEventListener("change", (event) => {
              nameText.setAttribute("value", event.target.value);
            });

            gridItems.appendChild(nameTextContainer);
            seats.appendChild(gridItems);
          }
        } else if (this.seats.num > configSeats) {
          let cnt = this.seats.num - configSeats;
          while (cnt > 0) {
            seats.removeChild(seats.children[this.seats.num - cnt]);
            cnt--;
          }
        }
        this.seats.num = configSeats;
      },
      deep: true,
    },
  },
  mounted() {
    for (let i = 0; i < this.num; i++) {
      this.students.push(`${i}さん`);
    }
    this.seats.num = this.num;

    for (let i = 0; i < this.seats.num; i++) {
      this.seats.can.push(true);
    }

    const main = document.querySelector("main");
    const grid = document.createElement("div");
    grid.setAttribute("id", "seats");
    grid.setAttribute("class", `grid grid-cols-${this.cols} gap-4`);

    for (let i = 0; i < this.seats.num; i++) {
      const gridItems = document.createElement("div");
      gridItems.setAttribute(
        "class",
        `border w-full h-10 bg-red-400 flex justify-center items-center`
      );

      const nameTextContainer = document.createElement("div");
      const nameText = document.createElement("input");
      nameText.setAttribute(
        "class",
        `w-full text-center bg-transparent border-opacity-0" type="text`
      );
      nameText.setAttribute("value", `${i}さん`);
      nameTextContainer.appendChild(nameText);
      nameTextContainer.setAttribute(
        "class",
        `white-space-nowrap p-2 text-indigo-700`
      );

      gridItems.addEventListener("click", () => {
        this.seats.can[i] = !this.seats.can[i];
        gridItems.classList.toggle("bg-red-400");
        gridItems.classList.toggle("bg-blue-400");
      });

      nameText.addEventListener("change", (event) => {
        nameText.setAttribute("value", event.target.value);
      });

      gridItems.appendChild(nameTextContainer);
      grid.appendChild(gridItems);
    }
    main.appendChild(grid);
  },
  methods: {
    changeSeat() {
      this.students = [];
      for (let i = 0; i < this.seats.num; i++) {
        if (this.seats.can[i]) {
          const textName = document.querySelector(
            `#seats > div:nth-child(${parseInt(i + 1)}) > div > input`
          );
          this.students.push(textName.getAttribute("value"));
        }
      }

      let i = null;
      let oldArray = this.students.slice();
      let n = oldArray.length;
      let newArray = [];
      while (n > 0) {
        i = Math.floor(Math.random() * n);
        newArray.push(oldArray[i]);
        oldArray.splice(i, 1);
        n = oldArray.length;
      }
      this.students = newArray;
      let cnt = 0;
      for (let i = 1; i <= this.seats.num; i++) {
        if (this.seats.can[i - 1]) {
          const textName = document.querySelector(
            `#seats > div:nth-child(${i}) > div > input`
          );
          textName.setAttribute("value", this.students[cnt]);
          textName.value = this.students[cnt];
          cnt++;
        }
      }
    },
  },
};
</script>

<style>
</style>