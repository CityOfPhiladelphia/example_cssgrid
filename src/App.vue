<template>
  <div id="app">
    <app-header
      :app-title="'test'"
      :is-sticky="true"
    />

    <main
      class="no-padding main-container"
      :style="{ 'grid-template-rows': gridRows }"
    >
      <div
        v-if="refinePanel"
        class="refine"
      >
        REFINE PANEL
      </div>

      <div class="menu">
        LOCATIONS PANEL
      </div>

      <div class="content">
        MAP PANEL
      </div>
    </main>

    <app-footer
      :is-sticky="true"
      :is-hidden-mobile="true"
    >
      <ul>
        <li>
          <a href="/about">About</a>
        </li>
        <li>
          <a href="/terms-and-conditions">Terms & Conditions</a>
        </li>
      </ul>
    </app-footer>

  </div>
</template>

<script>

import Vue from 'vue';

import {
  AppHeader,
  AppFooter,
  // MobileNav,
  // InputForm,
  // Textbox,
} from '@phila/phila-ui';

export default {
  name: 'App',
  components: {
    AppHeader,
    AppFooter,
    // MobileNav,
    // InputForm,
    // Textbox,
  },
  data() {
    return {
      dropdownOptions: {
        address: {
          text: 'Address',
          data: null,
        },
        keyword: {
          text: 'Keyword',
          data: null,
        },
      },
      refinePanel: true,
    };
  },
  computed: {
    gridRows() {
      let value;
      if (this.refinePanel) {
        value = '100px auto';
      } else {
        value = 'auto';
      }
      return value;
    }
  },
  watch: {
    isMobile(nextIsMobile) {
      if (nextIsMobile) {
        console.log('is mobile');
        this.handleResize();
      }
    },
    isTablet(nextIsTablet) {
      if (nextIsTablet) {
        console.log('is tablet');
        this.handleResize();
      }
    },
    isDesktop(nextIsDesktop) {
      if (nextIsDesktop) {
        console.log('is desktop');
        this.handleResize();
      }
    },
    isWideScreen(nextIsWidescreen) {
      if (nextIsWidescreen) {
        console.log('is widescreen');
        this.handleResize();
      }
    },
  },
  mounted() {
    this.handleResize();
  },
  methods: {
    handleResize () {
      console.log('handleResize is starting');
      //wait for dom to finish updating
      let isMobile = this.isMobile;
      Vue.nextTick(function () {
        let header = document.querySelector('#app-header');
        let footer = document.querySelector('#app-footer');
        let main = document.querySelector('main');
        let headerOffsetHeight = header.offsetHeight || 0;
        let footerOffsetHeight = 0;
        if (footer !== null) {
          footerOffsetHeight = footer.offsetHeight;
        }
        let offsetHeight;
        if (isMobile) {
          let offsetHeight = headerOffsetHeight;
          console.log('handleResize isMobile, offsetHeight:', offsetHeight, 'headerOffsetHeight:', headerOffsetHeight, 'footerOffsetHeight:', footerOffsetHeight);
          main.style['min-height'] = `calc(100vh - ${headerOffsetHeight}px)`;
        } else {
          let offsetHeight = headerOffsetHeight + footerOffsetHeight;
          console.log('handleResize is NOT mobile, offsetHeight:', offsetHeight, 'headerOffsetHeight:', headerOffsetHeight, 'footerOffsetHeight:', footerOffsetHeight);
          main.style['min-height'] = `calc(100vh - ${offsetHeight}px)`;
        }
      });
    },
  },
};

</script>

<style lang="scss">

@import "./assets/scss/main.scss";

// html {
//   box-sizing: border-box;
//   // height: 100%;
//   background-color: #ffeead;
//   margin: 0px;
// }
//
// body {
//   height: 100%;
// }

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // height: 100%;
  text-align: center;
  color: #000000;
}

.container {
  grid-gap: 1px;
}

.no-padding {
  padding: 0px !important;
}

.main-container {
  // height: 100%;
  display: grid;
  // padding: 0px;
  grid-gap: 1px;
  grid-template-columns: repeat(2, 1fr);
  // grid-template-rows: 40px auto 40px;
}

.main-container > div {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2em;
  color: #000000;
}

.header {
  background-color: #2176d2;
  grid-column: 1 / -1;
  // height: 30px;
}

.refine {
  background-color: #f0f0f0;
  grid-column: 1 / -1;
}


.menu {
  background-color: #88d8b0;
}

.content {
  background-color: #ff6f69;
}

.footer {
  background-color: #2176d2;
  grid-column: 1 / -1;
}


</style>
