<script setup lang="ts">
import TodoList from '@/components/Todo/TodoList.vue';

</script>

<template>
  <main>
    <h1 class="title">TODOアプリ</h1>
    <TodoList />
  </main>
</template>

<style lang="scss" scoped>
.title {
  font-size: 40px;
  font-weight: 700;
  color: white;
}
</style>
