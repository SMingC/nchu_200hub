<template>
  <div class="Team">
    <img :src="url" alt="teamAll" />
    <div
      :class="{ 'light-tangle': isDarkMode, 'dark-tangle': !isDarkMode }"
      class="NamesTangle"
    >
      <div class="Names">
        <div
          :class="{ 'light-text': isDarkMode, 'dark-text': !isDarkMode }"
          v-for="(name, index) in names"
          :key="index"
          class="name"
        >
          {{ name }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  props: ["url"],
  data() {
    return {
      names: ["MCS", "MHW", "ZZT"],
    };
  },
  computed: {
    isDarkMode() {
      return this.$store.getters.isDarkMode;
    },
  },
};
</script>

<style lang="scss" scoped>
.micleBird {
  top: -20vh;
  left: 20vw;
  @media (max-width: 900px) {
    top: -180px;
    left: -20px;
  }
}
.liner {
  top: 37vh;
  left: 28vw;
  @media (max-width: 900px) {
    top: 80px;
    left: -10px;
  }
}
.Team {
  display: grid;
  grid-auto-rows: auto auto;
  position: relative;
  @media (max-width: 900px) {
    grid-template-rows: auto auto;
    grid-template-columns: auto;
    justify-items: center;
    /* margin-left: -200px; */
    img {
      width: 200px;
    }
    img:hover {
      transform: scale(1.1) !important;
    }
    .name {
      font-size: 10px !important;
      line-height: 14px !important;
    }
  }

  img:hover {
    transform: scale(1.1) translateY(-20px);
    filter: hue-rotate(10deg) brightness(150%) saturate(120%);
  }
  .light-tangle {
    border: 4px solid #ffffff;
  }
  .dark-tangle {
    border: 4px solid #030f26;
  }
  .NamesTangle {
    width: 630px;
    padding: 10px;
    &:hover {
      transform: scale(1.1);
    }
    @media (max-width: 900px) {
      width: 200px;
    }
  }
  .Names {
    display: grid;
    grid-template-columns: repeat(3, auto);
    align-items: center;
    justify-items: center;
    .name {
      font-family: "Cascadia Code";
      font-style: normal;
      font-weight: 700;
      font-size: 27px;
      line-height: 27px;
      text-transform: uppercase;
    }
  }
}
</style>
