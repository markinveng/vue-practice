<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    isVisible: {
      type: Boolean,
      required: true,
    },
  },
  emits: ['close'],
  methods: {
    close() {
      this.$emit('close')
    },
  },
})
</script>

<template>
  <div v-if="isVisible" class="modal_overlay" @click.self="close">
    <div class="modal_content">
      <slot></slot>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.modal_overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal_content {
  background: #2c2c2c;
  padding: 20px;
  border-radius: 5px;
  min-width: 500px;
  max-width: 300px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}
</style>
