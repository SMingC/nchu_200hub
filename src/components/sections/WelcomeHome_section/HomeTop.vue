<template>
  <div class="WrapperHomeTop">
    <div class="Text">NCHU_200hub</div>
    <div class="ContentWrapperHomeTop">
      <div class="description">
        <div class="spring">多少游春意 🔥</div>
        <div
          :class="{ 'light-text': isDarkMode, 'dark-text': !isDarkMode }"
          class="intersting"
        >
          <span class="text">{{ showText }}</span>
        </div>
        <div class="poem">
          <div
            :class="{ 'light-text': isDarkMode, 'dark-text': !isDarkMode }"
            class="poemText"
          >
            寸心誓与长相守
          </div>
          <div
            :class="{ 'light-text': isDarkMode, 'dark-text': !isDarkMode }"
            class="poemText"
          >
            止华凄冷蓼花愁
          </div>
          <div
            :class="{ 'light-text': isDarkMode, 'dark-text': !isDarkMode }"
            class="poemText"
          >
            挑尽寒灯到夜深
          </div>
          <div
            :class="{ 'light-text': isDarkMode, 'dark-text': !isDarkMode }"
            class="poemText"
          >
            战和何者是良筹
          </div>
        </div>
        <div v-show="isOnComputer" class="HomeButtonWrapperHomeTop">
          <HomeButton
            :isColorReversed="false"
            description="让我康康"
            where="/video"
          />
          <HomeButton
            :isColorReversed="true"
            description="一些课程"
            where="/team"
          />
        </div>
      </div>
      <TeamAll :url="cartoonUrls[0].teamCard.url" class="TeamAll" />
      <HomeButton
        v-show="!isOnComputer"
        :isColorReversed="false"
        description="让我康康"
        where="/video"
        class="onPhonButton"
      />
      <HomeButton
        v-show="!isOnComputer"
        :isColorReversed="true"
        description="一些课程"
        where="/team"
        class="onPhonButton"
      />
    </div>
    <div class="CardsWrapperHomeTop">
      <div
        :class="{ 'light-text': isDarkMode, 'dark-text': !isDarkMode }"
        class="cardTitle"
      >
        TEAM sofar...
      </div>
      <div class="Cards">
        <CartoonCard
          v-for="(card, index) in cartoonUrls[0].cartoonCardsCollection.items"
          :key="index"
          :url="card.url"
        />
      </div>
    </div>
  </div>
</template>

<script>
import HomeButton from "@/components/button/HomeButton.vue";
import CartoonCard from "@/components/group/welcomeHome/SomethingIntersting_Page/CartoonCards.vue";
import TeamAll from "@/components/group/welcomeHome/SomethingIntersting_Page/TeamAllPage.vue";
export default {
  components: {
    HomeButton,
    CartoonCard,
    TeamAll,
  },
  async created() {
    this.cartoonUrls = await this.getcartoonUrl();
    if (document.body.clientWidth <= 900) {
      this.isOnComputer = false;
    }
    this.changeText();
  },
  watch: {
    showText() {},
  },
  methods: {
    getcartoonUrl: async () => {
      const query = `{
        nchu200HubCollection {
          items{
            cartoonCardsCollection{
              items{
                url
              }
            }
            teamCard{
              url
            }
          }
        }
      }`;
      const fetchUrl = `https://graphql.contentful.com/content/v1/spaces/${process.env.VUE_APP_CONTENTFUL_SPACE_ID}/`;

      const fetchOptions = {
        method: "POST",
        headers: {
          Authorization: `Bearer ${process.env.VUE_APP_CONTENTFUL_ACCESS_TOKEN}`,
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ query }),
      };

      try {
        const response = await fetch(fetchUrl, fetchOptions).then((response) =>
          response.json()
        );
        return response.data.nchu200HubCollection.items;
      } catch (error) {
        throw new Error("Could not receive the data from Contentful!");
      }
    },
    changeText() {
      var text = [
        "灯火万家城四畔,星河一道水中央",
        "十里寒潭路,烟花一半醒",
        "城上春雨覆宛墙,江亭晚色静年芳",
        "至今窥牧马，不敢过临洮",
        "若耶溪傍采莲女，笑隔荷花共人语",
        "胜地不常，盛筵难再",
        "兰亭已矣，梓泽丘墟",
        "铁马红旗寒日暮，使君犹寄边城",
        "中秋谁与共孤光。把盏凄然北望",
        "一夜风雨，江南好风光",
      ];
      var index = 0,
        temp = 0,
        toggle = true;

      setInterval(() => {
        if (toggle) {
          this.showText = text[temp].slice(0, ++index);
        } else {
          this.showText = text[temp].slice(0, index--);
        }

        if (index == text[temp].length + 3) {
          toggle = false;
        } else if (index == 0) {
          toggle = true;
          temp++;
          if (temp >= text.length) {
            temp = 0;
          }
        }
      }, 120);
    },
    beforeDestroy() {
      clearInterval(this.changeText);
    },
  },

  data() {
    return {
      isOnComputer: true,
      cartoonUrls: [],
      showText: null,
    };
  },
  computed: {
    isDarkMode() {
      return this.$store.getters.isDarkMode;
    },
  },
};
</script>

<style scoped lang="scss">
@import "@/global-styles/colors.scss";
@import "@/global-styles/typography.scss";

.text {
  display: inline-block;
  position: relative;
}
.text::after {
  content: "";
  position: absolute;
  right: -10px;
  top: 5px;
  height: 50px;
  width: 3px;
  background-color: #fff;
  animation: san 1s steps(1) infinite;
  @media (max-width: 900px) {
    height: 25px;
    width: 1px;
  }
}
@keyframes san {
  0%,
  100% {
    background-color: #fff;
  }
  50% {
    background-color: transparent;
  }
}

.onPhonButton {
  transform: scale(0.8);
  margin-top: 40px;
}
.TeamAll {
  @media (max-width: 900px) {
    margin-top: 180px;
  }
}
@import "@/global-styles/mixin.scss";
@keyframes Op {
  from {
    opacity: 0;
    transform: translateY(-10px);
    filter: blur(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0px);
    filter: blur(0px);
  }
}
.WrapperHomeTop {
  padding: 40px;
  @media (max-width: 900px) {
    padding: 20px;
  }

  .Text {
    font-family: "HanziPen SC";
    font-style: normal;
    font-weight: 400;
    font-size: 24px;
    line-height: 34px;
    color: #030f26;
    @media (max-width: 900px) {
      font-weight: 400;
      font-size: 20px;
      line-height: 24px;
    }
  }

  .ContentWrapperHomeTop {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    align-items: center;
    margin-top: 10vh;
    padding-left: 5vw;
    position: relative;
    @media (max-width: 900px) {
      grid-template-columns: repeat(2, 130px);
    }

    .description {
      display: grid;
      grid-template-rows: repeat(4, auto);
      padding-top: 5vh;
      gap: 5vh;
      justify-items: start;
      align-items: center;

      opacity: 0;
      animation: Op 1s forwards;
      .spring {
        font-family: "HanziPen SC";
        font-style: normal;
        font-weight: 400;
        font-size: 16px;
        line-height: 22px;
        color: #ffffff;
        padding: 15px 10px;
        text-align: center;
        background: #090017;
        border-radius: 29px;
        animation: Op 1s forwards;
        opacity: 0;
        animation-delay: 0.2s;
      }

      .intersting {
        font-family: "HanziPen SC";
        font-style: normal;
        font-weight: 700;
        font-size: 35px;
        line-height: 58px;
        animation: Op 1s forwards;
        animation-delay: 0.4s;
        opacity: 0;
        @media (max-width: 900px) {
          font-size: 15px;
          line-height: 40px;
          width: 500px;
        }
      }

      .poem {
        opacity: 0;
        background: rgba(15, 14, 71, 0.3);
        border: 0.5px solid rgba(255, 255, 255, 0.2);
        box-shadow: 0px 50px 100px rgba(0, 0, 0, 0.25);
        backdrop-filter: blur(40px);
        border-radius: 20px;
        padding: 30px;
        @media (max-width: 900px) {
          overflow-x: hidden;
        }

        text-align: center;
        animation: Op 1s forwards;
        animation-delay: 0.6s;
        @media (max-width: 900px) {
          padding: 10px;
        }

        .poemText {
          font-family: "HanziPen SC";
          font-style: normal;
          font-weight: 400;
          font-size: 24px;
          line-height: 46px;
          &:hover {
            cursor: defalut;
            transform: translateX(20px);
          }
          @media (max-width: 900px) {
            font-size: 10px;
          }
        }
      }

      .HomeButtonWrapperHomeTop {
        display: grid;
        grid-template-columns: auto auto;
        gap: 50px;
        animation: Op 1s forwards;
        animation-delay: 0.8s;
        opacity: 0;
        /* @media (max-width: 900px) {
          grid-template-columns: auto;
          grid-template-rows: auto auto;
        } */
      }
    }
  }

  .CardsWrapperHomeTop {
    margin-top: 10vh;
    padding-left: 5vw;
    .cardTitle {
      font-family: "Cascadia Code";
      font-style: normal;
      font-weight: 700;
      font-size: 40px;
      line-height: 58px;
      letter-spacing: 0.5px;
      @media (max-width: 900px) {
        font-size: 20px;
      }
    }
    .Cards {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      align-items: center;
      justify-items: center;
      @media (max-width: 900px) {
        grid-template-rows: repeat(4, auto);
        grid-template-columns: auto;
      }
    }
  }
}
</style>
