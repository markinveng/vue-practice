<script lang="ts">
import { defineComponent, type PropType } from 'vue'

export default defineComponent({
  props: {
    title: {
      type: String as PropType<string>, // プロパティの型を文字列に指定
      required: true, // プロパティを必須に設定
    },
  },
})
</script>

<template>
  <h1 class="pageTitle">{{ title }}</h1>
</template>

<style lang="scss" scoped>
@use '@/assets/mixin.scss';
.pageTitle {
  font-size: 40px;
  font-weight: 700;
  color: white;
  @include mixin.sp {
    text-align: center;
  }
}
</style>
