<template>
  <li class="chip_container">
    <div class="corner chip" :class="_class">
      <!-- Chip Image -->
      <div class="game_pic">
        <p class="title">{{name}}</p>
        <img
          :src="previewUrl"
          class="w-full h-[119px] rounded-t"
        />
        <p
          class="text-center font-bold absolute right-[6px] bottom-0 drop-shadow-[2px_2px_0px_rgb(0,0,0)]"
          style="color: rgb(255, 255, 125)"
        >
          {{damage}}
        </p>
      </div>

      <!-- Chip C -->
      <div class="chip_inner">
        <p class="chip_codes">
          <img
            class="h-[14px] w-[14px] mr-2"
            :src="iconUrl"
          />
          {{code}}
        </p>
      </div>

      <!-- Description -->
      <div class="description">
        {{description}}
      </div>

      <!--Chip Teeths  -->
      <div class="chip-teeth">
        <span></span>
        <span></span>
        <span></span>
        <span></span>
        <span></span>
        <span></span>
        <span></span>
        <span></span>
        <span></span>
        <span></span>
        <span></span>
      </div>

      <!-- Info's -->
      <div class="detail_view" style="display: none">
        <p>ID: com.DJ.card.DarkLegnofAcrius</p>
        <p>Uploader: Cyanman EXE</p>
        <p>
          Thread:
          <a
            href="discord://discord.com/channels/455429604455219211/1006720904895664158/1006720908473421946"
            target="_blank"
            class="text-[rgb(252,255,0)]"
            >Discord</a
          >
          <a
            href="https://discord.com/channels/455429604455219211/1006720904895664158/1006720908473421946"
            target="_blank"
            class="text-[rgb(252,255,0)]"
            >Web</a
          >
        </p>
        <p>Date: 8/10/2022</p>
        <p>Hash: 8048cd2dec104fdc5759723e03a1faad</p>
        <p>Tags: [CUS] [CHIP]</p>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  name: "ChipWidget",

  components: {},

  computed: {
    isMega() {
      return this.type.toLowerCase() == "mega";
    },

    isReg() {
      return this.type.toLowerCase() == "regular";
    },

    isGiga() {
      return this.type.toLowerCase() == "giga";
    },

    isDark() {
      return this.type.toLowerCase() == "dark";
    },

    _class() {
      let result = "chip-box ";

      if (this.isMega) {
        result += "Mega";
      } else if (this.isGiga) {
        result += "Giga";
      } else if (this.isDark) {
        result += "Dark";
      } /* this.isReg */ else {
        result += "Standard";
      }

      return result;
    },
  },

  props: {
    type: {
      default: "regular",
      type: String,
    },
    name: {
      type: String,
    },
    damage: {
      type: String,
    },
    description: {
      type: String,
    },
    code: {
      type: String,
    },
    iconUrl: {
      type: String,
    },
    previewUrl: {
      type: String
    }
  },

  data() {
    return {};
  },
};
</script>
