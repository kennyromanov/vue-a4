<script lang="ts">

import { defineComponent, ref } from 'vue';
import { Obj, NullablePropType } from '@/types';
import Paged from 'vue-paged';


// Constants

export const DEFAULT_PAPER_WIDTH = 814;


export default defineComponent({
  name: 'VueA4',

  components: { Paged },
  
  props: {
    paperWidth: {
      type: Number as NullablePropType<number>,
      default: null
    },

    noTheme: {
      type: Boolean as NullablePropType<boolean>,
      default: false
    },
  },

  setup() {
    const containerEl = ref<any>(null);
    const innerEl = ref<any>(null);
    const containerWidth = ref<number|null>(null);

    return { containerEl, innerEl, containerWidth };
  },

  created() {
    this.useResize();
    this.updContainerSize();
  },

  mounted() {
    this.updContainerSize();
  },

  updated() {
    this.updContainerSize();
  },

  beforeUnmount() {
    this.remResize();
  },

  methods: {
    useResize() {
      window.addEventListener('resize', this.updContainerSize.bind(this));
    },
    remResize() {
      window.removeEventListener('resize', this.updContainerSize);
    },
    updContainerSize() {
      this.containerWidth = this.containerEl?.offsetWidth ?? null;
    },
  },

  computed: {
    innerScale(): number|null {
      const paperWidth = this.paperWidth ?? DEFAULT_PAPER_WIDTH;

      if (this.containerWidth)
        return this.containerWidth / paperWidth;
      else
        return null;
    },
    innerStyle(): Obj | null {
      if (this.innerScale)
        return { transform: `scale(${this.innerScale})` };
      else
        return null;
    },
  },
});

</script>

<template>
  <div class="vue_a4" ref="containerEl" :data-theme="Number(!noTheme)">
    <div class="vue_a4_inner" :style="innerStyle" ref="innerEl">
      <Paged>
        <slot />
      </Paged>
    </div>
  </div>
</template>