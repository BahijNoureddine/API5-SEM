<template>
  <router-view />
</template>

<script lang="ts">
import { defineComponent, nextTick, onMounted } from "vue";
import { useStore } from "vuex";
import { Mutations } from "@/store/enums/StoreEnums";
import { initializeComponents } from "@/core/plugins/keenthemes";
import { GChart } from "vue-google-charts";


export default defineComponent({
  name: "app",
  components: {
    GChart
  },
  setup() {
    const store = useStore();


    onMounted(() => {
      /**
       * this is to override the layout config using saved data from localStorage
       * remove this to use config only from static config (@/core/config/DefaultLayoutConfig.ts)
       */
      store.commit(Mutations.OVERRIDE_LAYOUT_CONFIG);

      nextTick(() => {
        initializeComponents();
      });
    });
  },
});
</script>

<style lang="scss">
@import "~bootstrap-icons/font/bootstrap-icons.css";
@import "~apexcharts/dist/apexcharts.css";
@import "~quill/dist/quill.snow.css";
@import "~animate.css";
@import "~sweetalert2/dist/sweetalert2.css";
@import "~nouislider/distribute/nouislider.css";
@import "~@fortawesome/fontawesome-free/css/all.min.css";
@import "~socicon/css/socicon.css";
@import "~line-awesome/dist/line-awesome/css/line-awesome.css";
@import "~dropzone/dist/dropzone.css";
@import "~@vueform/multiselect/themes/default.css";
@import "~prism-themes/themes/prism-shades-of-purple.css";
@import "~element-plus/dist/index.css";
@import "assets/sass/plugins.dark";
@import "assets/sass/style.dark";

// Main demo style scss
@import "assets/sass/plugins";
@import "assets/sass/style";

// Dark mode demo style scss
//@import "assets/sass/plugins.dark";
//@import "assets/sass/style.dark";

//RTL version styles
//@import "assets/css/style.rtl.css";
</style>
