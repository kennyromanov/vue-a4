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
    const observer = ref<any>(null);
    const containerWidth = ref<number|null>(null);
    const innerHeight = ref<number|null>(null);

    return {
      containerEl,
      innerEl,
      observer,
      containerWidth,
      innerHeight,
    };
  },

  mounted() {
    this.useObserver();
    this.updSize();
    this.updInnerSize();
  },

  updated() {
    this.updSize();
    this.updInnerSize();
  },

  beforeUnmount() {
    this.remObserver();
  },

  methods: {
    useObserver() {
      this.observer = new ResizeObserver(() => {
        this.updSize();
        this.updInnerSize();
      });

      this.observer?.observe(this.containerEl, this.innerEl);
    },
    remObserver() {
      this.observer?.unobserve(this.containerEl, this.innerEl);
    },
    updSize() {
      const containerRect = this.containerEl?.getBoundingClientRect() ?? null;
      this.containerWidth = containerRect?.width ?? null;
    },
    updInnerSize() {
      const innerRect = this.innerEl?.getBoundingClientRect() ?? null;
      this.innerHeight = innerRect?.height ?? null;
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
    style(): Obj | null {
      if (this.innerHeight)
        return { height: this.innerHeight + 'px' };
      else
        return null;
    },
    innerStyle(): Obj | null {
      if (this.innerScale)
        return { width: 0, transform: `scale(${this.innerScale})` };
      else
        return null;
    },
  },
});

</script>

<template>
  <div class="vue_a4" :style="style" :data-theme="Number(!noTheme)" ref="containerEl">
    <div class="vue_a4_inner" :style="innerStyle" ref="innerEl">
      <Paged>
        <slot />
      </Paged>
    </div>
  </div>
</template>