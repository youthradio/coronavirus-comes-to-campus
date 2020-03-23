<template>
  <div class="container">
    <MenuHeader :toggle-enable="false" />
    <div class="grid">
      <div class="grid-col-left">
        <div class="sticky-top">
          <h1> {{ articleData.title }} </h1>
          <div v-html="articleData.intro.text" />

          <MapContainer
            class="map-container"
            :active-state="activeState"
          />
        </div>
      </div>
      <div class="grid-col-right">
        <SocialEmbeds
          :embeds-data="embedsData"
          @onActiveState="onActiveState"
          @onSelectCategory="c => selectedCategory = c"
        />
      </div>
      <div class="grid-row">
        <h2>{{ articleData.resources.title }}</h2>
        <div
          class="multi-col"
          v-html="articleData.resources.text"
        />
      </div>
      <div class="grid-row credits">
        <h2>{{ articleData.credits.title }}</h2>
        <div v-html="articleData.credits.text" />
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

<style lang="scss" scoped>
@import "~@/css/vars";
@import "~@/css/mixins";

.grid {
  display: grid;
  grid-template-columns: minmax(100%, 1fr);
  @include breakpoint(medium) {
    grid-template-columns: 1fr minmax(320px, 1fr);
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
    position: sticky;
    top: 68px; // menu height
  }
}

.map-container {
  touch-action: none;
  pointer-events: none;
  z-index: 10000;
  position: fixed;
  width: 60%;
  bottom: 50px;
  right: 0px;
  @include breakpoint(medium) {
    position: inherit;
    width: 100%;
  }
}

.credits {
  background-color: $grey;
}
</style>
