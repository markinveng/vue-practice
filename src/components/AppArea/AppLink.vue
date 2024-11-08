<script lang="ts">
import { defineComponent, ref } from 'vue'

export interface App {
  id: number
  name: string
  path: string
  description: string
}

const appList = ref<App[]>([
  {
    id: 1,
    name: 'TODOアプリ',
    path: '/todo',
    description:
      'CRUD機能を備えたTODOアプリです。データはローカルホストに保存されます。',
  },
  {
    id: 2,
    name: 'お天気アプリ',
    path: '/wether',
    description: '気象庁のAPIから天気を表示するアプリです。(未完)',
  },
])

export default defineComponent({
  setup() {
    return {
      appList,
    }
  },
})

export { appList }
</script>

<template>
  <div class="appLink_wrapper">
    <li v-for="item in appList" :key="item.id" class="appLink_item">
      <RouterLink :to="item.path" class="appLink_link">
        <h2 class="appLink_title green">{{ item.name }}</h2>
        <p class="appLink_description">{{ item.description }}</p>
      </RouterLink>
    </li>
  </div>
</template>

<style lang="scss" scoped>
.appLink {
  &_wrapper {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 30px;
  }

  &_item {
    width: 180px;
    border-radius: 20px;
    padding: 20px;
    &:hover {
      background-color: hsla(160, 100%, 37%, 0.2);
      transition-duration: 0.7s;
    }
  }

  &_title {
    font-size: 20px;
    font-weight: 700;
  }

  &_description {
    color: white;
    font-weight: 500;
  }
}
</style>
