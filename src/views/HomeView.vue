<script setup lang="ts">
import AppLink from '@/components/AppArea/AppLink.vue'
import PageTitle from '@/components/Common/PageTitle.vue';
</script>

<template>
  <main>
    <PageTitle title="アプリ一覧" />
    <AppLink />
  </main>
</template>

<style lang="scss" scoped>
@use '@/assets/mixin.scss';
.title {
  font-size: 40px;
  font-weight: 700;
  color: white;
  @include mixin.sp {
    text-align: center;
  }
}
</style>
