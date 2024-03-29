<template>
  <div class="container">
    <MenuHeader :toggle-enable="false" />
    <div class="grid">
      <div class="grid-col-left">
        <div class="sticky-top">
          <h1> {{ articleData.title }} </h1>
          <div class="float">
            <div class="bottom-container">
              <MapContainer
                class="map-container"
                :active-state="activeState"
              />
              <div class="more">
                <span><a href="#resources">Additional Resources</a></span>
                <span><a href="#credits">Credits</a></span>
              </div>
            </div>
            <div
              class="intro"
              v-html="articleData.intro.text"
            />
          </div>
        </div>
      </div>
      <div class="grid-col-right">
        <SocialEmbeds
          v-if="embedsData"
          :embeds-data="embedsData"
          @onActiveState="onActiveState"
          @onSelectCategory="c => selectedCategory = c"
        />
      </div>
      <div
        id="resources"
        class="grid-row resources"
      >
        <h2>{{ articleData.resources.title }}</h2>
        <div
          class="multi-col"
          v-html="articleData.resources.text"
        />
      </div>
      <div
        id="credits"
        class="grid-row credits"
      >
        <h2>{{ articleData.credits.title }}</h2>
        <template v-for="row in articleData.credits.people">
          <div :key="row.title">
            <span class="title">{{ row.title }}: </span>{{ row.names }}
          </div>
        </template>
      </div>
    </div>
    <ShareContainer />
    <FooterContainer />
  </div>
</template>

<script>

import CommonUtils from '../mixins/CommonUtils'
import ArticleData from '../data/data.json'
import Embeds from '../data/embeds.json'
import SocialEmbeds from '~/components/Custom/SocialEmbeds'
import MapContainer from '~/components/Custom/Map/MapContainer'
import MenuHeader from '~/components/Header/MenuHeader'
import ShareContainer from '~/components/custom/ShareContainer'
import FooterContainer from '~/components/Footer/FooterContainer'

export default {
  components: {
    MapContainer,
    MenuHeader,
    SocialEmbeds,
    ShareContainer,
    FooterContainer
  },
  mixins: [
    CommonUtils
  ],
  asyncData (ctx) {
    return {
      articleData: ArticleData.content,
      embedsData: Embeds
    }
  },
  data () {
    return {
      activeState: null,
      selectedCategory: null
    }
  },
  computed: {},
  watch: {

  },
  methods: {
    onActiveState ({ state, location, isVisible }) {
      this.activeState = Object.assign({}, { state, location, isVisible, color: this.selectedCategory.color })
    }

  }
}
</script>

<style lang="scss">
@import "~@/css/vars";
@import "~@/css/mixins";
@import "~@/css/base";

p {
  // line-height: 1.25rem;
}

.grid {
  margin-left: auto;
  margin-right: auto;
  max-width: 1200px;
  display: grid;
  grid-template-columns: minmax(100%, 1fr);
  @include breakpoint(medium) {
    grid-template-columns: 2fr minmax(450px, 1fr);
  }
}
.grid-col-left {
  padding: 0 1rem 0 1rem;
  grid-column: 1;
  grid-row: auto;
}
.grid-col-right {
  padding: 0 1rem 0 1rem;
  grid-column: 1;
  grid-row: auto;
  @include breakpoint(medium) {
    grid-column: 2;
  }
}
.grid-row {
  padding: 0 1rem 0 1rem;
  grid-row: auto;
  grid-column: 1;
  @include breakpoint(medium) {
    grid-column: 1 / 3;
  }
}
.multi-col {
  column-count: 1;
  column-gap: 1rem;
  @include breakpoint(medium) {
    column-count: 2;
  }
}
.sticky-top {
  @include breakpoint(medium) {
    height: calc(100vh - 68px);
    position: sticky;
    top: 68px; // menu height
  }
}

.float::before {
  content: " ";
  width: 0px;
  float: right;
  margin-top: 30%;
  @media screen and (max-height: 750px) {
    margin-top: 0px;
  }
}
.bottom-container {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  touch-action: none;
  z-index: 10000;
  float: right;
  clear: right;
  width: 60%;

  @media screen and (orientation: portrait) {
    width: 60%;
    @media (min-width: 768px) {
      width: 100%;
    }
  }
  @media screen and (orientation: landscape) {
    @media (min-height: 600px) {
      width: 50%;
    }
    @media (min-height: 750px) {
      width: 60%;
    }
    @media (min-height: 850px) {
      width: 70%;
    }
    @media (min-height: 1000px) {
      width: 100%;
    }
  }

  margin-top: 0.5rem;
  position: fixed;
  bottom: 20px;
  right: 0px;
  background-color: rgba(#c4c4c4, 0.5);
  border-radius: 20px;
  padding: 0.5rem;
  @include breakpoint(medium) {
    background-color: unset;
    position: inherit;
  }
}
.intro {
  display: inline;
}
.map-container {
  touch-action: none;
  pointer-events: none;
  z-index: 10000;
}
.more {
  display: flex;
  justify-content: center;
  line-height: 1rem;
  font-size: 0.8rem;
  margin-top: auto;
  @media (min-height: 800px) {
    font-size: 1rem
  }
  span {
    margin: 1rem;
  }
}
.resources {
  padding-bottom: 1rem;
  margin-bottom: 1rem;
}
.credits {
  padding-bottom: 1rem;
  margin-bottom: 1rem;
  .title {
    font-weight: bold;
  }
}
</style>
