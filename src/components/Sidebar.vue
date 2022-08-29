<template>
  <div class="relative min-h-screen mt-10 w-[200px] pt-4 md:pt-8 bg-white">
    <h1 class="text-2xl font-bold text-center border-b">Filters</h1>
    <div class="filters">
      <li
        class="w-full py-2 border-b cursor-pointer shadow-[inset_0px_2px_4px_1px_rgba(0,0,0,0.1)]"
        style="--animation-delay: 1"
        :class="!isSidebar ? 'slideIn' : 'slideOut'"
      >
        <input
          type="text"
          placeholder="Search..."
          class="outline-none focus:outline-none"
          v-model="text_filter"
          @keyup="handleSearchChange()"
        />
      </li>
      <li
        v-for="(item, idx) in sidebarList"
        :key="idx"
        class="item"
        :style="sidebarStyle(idx + 2)"
        :class="!isSidebar ? 'slideIn' : 'slideOut'"
      >
        {{ item }}
      </li>
    </div>
  </div>
</template>

<script>
export default {
  name: "SideBar",
  data() {
    return {
      text_filter: "",
      sidebarList: ["Item 1", "Item 2", "Item 3", "Item 4"],
    };
  },
  methods: {
    sidebarStyle(idx) {
      return "--animation-delay: " + idx + ";";
    },
    handleSearchChange() {
      this.$emit("search", this.text_filter);
    }
  },
  props: {
    isSidebar: {
      default: false,
      type: Boolean,
    },
  },
};
</script>

<style>
.slideIn {
  animation: slideIn 300ms both ease-in;
  animation-delay: calc(var(--animation-delay) * 200ms);
}

.slideOut {
  animation: slideOut 500ms both ease-out;
  animation-delay: calc(var(--animation-delay) * 150ms);
}

@keyframes slideIn {
  0% {
    transform: translateX(0px);
  }
  100% {
    transform: translateX(-200px);
  }
}
@keyframes slideOut {
  0% {
    transform: translateX(-200px);
  }
  100% {
    transform: translateX(0px);
  }
}
</style>
