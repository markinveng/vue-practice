<script setup lang="ts">
import { RouterView } from 'vue-router'
</script>

<template>
  <header>
    <nav class="navigation">
      <RouterLink to="/" class="navigation_item">Home</RouterLink>
      <RouterLink to="/todo" class="navigation_item">Todo</RouterLink>
    </nav>
  </header>
  <RouterView />
</template>

<style lang="scss" scoped>
@use '@/assets/mixin.scss';
.navigation {
  display: flex;
  padding-top: 20px;
  gap: 20px;
  &_item {
    color: hsl(160, 100%, 37%);
    font-size: 20px;
    font-weight: 700;
    background-color: hsla(160, 100%, 37%, 0.2);
    padding: 10px;
    border-radius: 16px;
    &:hover {
      transition: 0.7;
      background: transparent;
    }
  }
  @include mixin.sp {
    justify-content: center;
  }
}
</style>
