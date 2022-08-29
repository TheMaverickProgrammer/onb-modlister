<template>
  <div id="app">
    <!-- higsby -->
    <div class="fixed bottom-0 z-50">
      <img
        src="@/assets/images/hman.png"
        alt="hman"
        class="fixed bottom-0 -scale-x-100 translate-y-[400px] left-[-100px] hidden md:block hman_comeIn"
      />
      <!-- TODO: improve dialog box for site news -->
      <!--<div
        class="msg_box_outer -rotate-4 msg_box_slideIn"
        style="display: none"
      >
        <div class="-mt-5 rotate-4 msg_box_inner">
          <div class="message">Message Box</div>
        </div>
      </div>-->
    </div>

    <!-- Navbar -->
    <div class="header">
      <button class="hamburgerIcon" @click.prevent="toggle()">
        <!-- 3 divs are needed for the hamburger -->
        <div :class="[!isSidebar ? '' : 'active']"></div>
        <div :class="[!isSidebar ? '' : 'active']"></div>
        <div :class="[!isSidebar ? '' : 'active']"></div>
      </button>
      <h1 class="text-sm md:text-[2.5rem] md:p-2">Higsby's Clearance Bin</h1>
    </div>

    <div class="flex">
      <div class="flex" :class="[!isSidebar ? 'slideIn' : ' slideOut']">
        <Sidebar :isSidebar="isSidebar" @search="handleSearchChange" />
      </div>
      <!-- Content -->
      <div class="pt-16">
        <div>
          <draggable
            ghost-class="ghost"
            group="mods"
            :list="mods"
            @change="handleCheckout"
            @start="handleDragStart"
            @end="handleDrop"
            class="available_bin"
          >
            <chip-widget
              v-for="(mod, idx) in filteredChips"
              :key="idx"
              :type="mod.type"
              :title="mod.title"
              :damage="mod.damage"
              :description="mod.description"
              :code="mod.code"
              :previewUrl="mod.previewUrl"
              :iconUrl="mod.iconUrl"
              :style="animationOrder(idx)"
            />
          </draggable>
        </div>
      </div>
    </div>
    <div class="shopping_bin">
      <!--<div class="msg_box_dock top-[-40px]">Message Box</div>-->

      <draggable
        ghost-class="ghost"
        group="mods"
        :list="checkout"
        @change="handleRestore"
        @start="handleDragStart"
        class="bin"
        :class="dropAnim"
      >
      <chip-widget
              v-for="(mod, idx) in checkout"
              :key="idx"
              :type="mod.type"
              :title="mod.title"
              :damage="mod.damage"
              :description="mod.description"
              :code="mod.code"
              :previewUrl="mod.previewUrl"
              :iconUrl="mod.iconUrl"
              :style="animationOrder(idx)"
            />
      </draggable>

      <button class="download_btn">Download</button>
      
      <button class="random_btn" @click="handleRandomPack">Random</button>
    </div>
  </div>
</template>

<script>
import ChipWidget from "@/components/ChipWidget.vue";
import Sidebar from "@/components/Sidebar.vue";

import draggable from "vuedraggable";

export default {
  name: "App",
  components: {
    ChipWidget,
    draggable,
    Sidebar,
  },

  data() {
    return {
      mods: this.populateDummyData(),
      checkout: [],
      sfx: {
        drop: new Audio(require("@/assets/sounds/drop.mp3")),
        pickup: new Audio(require("@/assets/sounds/pickup.mp3")),
      },
      dropAnim: "",
      isSidebar: false,
      searchFilter: "",
    };
  },

  mounted() {

  },

  computed: {
    filteredChips() {
      let regex = new RegExp(`${this.searchFilter}.*`);
      return this.mods.filter(x => regex.test(x.name));
    }
  },

  methods: {
    populateDummyData() {
      let arr = [];

      for(let i = 0; i < 100; i++) {
        arr[i] = {name: 'Test', description: 'blah blah blah', type: this.randomType(), damage: '888', code: 'Z', previewUrl: "https://metaperficient.com/mods.battlenetwork.io/game1.jpg", iconUrl: "http://mods.battlenetwork.io/images/com_DJ_card_DarkLegnofAcrius_icon.png"};
      }

      return arr;
    },

    handleCheckout(evt) {
      console.log(evt);
      this.checkout.push(evt.removed.element);
      this.sfx.drop.play();
    },

    handleRestore(evt) {
      console.log(evt);
      this.mods.push(evt.added.element);
      this.sfx.drop.play();
    },

    handleDragStart() {
      this.sfx.pickup.play();
    },

    handleDrop() {
      console.log("Dropped");
      this.dropAnim = "drop_anim";
      const vm = this;
      setTimeout(1000, () => {
        vm.dropAnim = "";
      }); // reset the variable after 1000 ms (1 second)
    },

    randomType() {
      const types = ["Giga", "Mega", "Dark", "Standard"];
      return types[Math.floor(Math.random() * types.length)];
    },

    toggle() {
      this.isSidebar = !this.isSidebar;
    },
    animationOrder(idx) {
      return "--animation-order: " + idx + ";";
    },

    handleSearchChange(data) {
      this.searchFilter = data;
    },

    handleRandomPack() {
      let idx = Math.random()*this.mods.length;
      this.checkout.push(...this.mods.splice(idx, 1));
    }
  },
};
</script>

<style>
.slideIn {
  transform: translateX(-300px);
  transition: all 250ms ease-in-out;
  display: none;
  margin-right: -150px;
}

.slideOut {
  transform: translateX(0);
  transition: all 250ms ease-in-out;
  display: block;
  margin-right: 10px;
}

.hman_comeIn {
  animation: comeIn 3s;
}

@keyframes comeIn {
  0% {
    left: -1000px;
  }
  100% {
    left: -100px;
  }
}
.msg_box_slideIn {
  animation: msg_slideIn 1s;
}

@keyframes msg_slideIn {
  0% {
    opacity: 0;
    transform: translateX(-200px);
  }
  100% {
    opacity: 1;
    transform: translateX(0);
  }
}
</style>
