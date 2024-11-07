<script lang="ts">
import { defineComponent, ref, onMounted, watch } from 'vue'
import type { Todo } from '@/components/Todo/Todo.types'
import EditModal from '@/components/Todo/EditModal.vue'

export default defineComponent({
  components: {
    EditModal,
  },
  setup() {
    const todos = ref<Todo[]>([])
    const newTodoText = ref('')
    const editingTodoId = ref<number | null>(null)
    const editTodoText = ref('')
    const isEditModalVisible = ref(false)

    // ローカルストレージからTodoリストを読み込む
    const loadTodos = () => {
      const storedTodos = localStorage.getItem('todos')
      todos.value = storedTodos ? JSON.parse(storedTodos) : []
    }

    // Todoリストをローカルストレージに保存
    const saveTodos = () => {
      localStorage.setItem('todos', JSON.stringify(todos.value))
    }

    // タスクの追加
    const addTodo = () => {
      if (newTodoText.value.trim() !== '') {
        todos.value.push({
          id: Date.now(),
          text: newTodoText.value,
          completed: false,
        })
        newTodoText.value = ''
        saveTodos()
      }
    }

    // タスクの削除
    const deleteTodo = (id: number) => {
      todos.value = todos.value.filter(todo => todo.id !== id)
      saveTodos()
    }

    // モーダルの表示
    const openEditModal = (todo: Todo) => {
      editingTodoId.value = todo.id
      editTodoText.value = todo.text
      isEditModalVisible.value = true
    }

    //モーダルを閉じる
    const closeEditModal = () => {
      editingTodoId.value = null
      editTodoText.value = ''
      isEditModalVisible.value = false
    }

    // タスクの編集開始
    const editTodo = (id: number) => {
      const todo = todos.value.find(todo => todo.id === id)
      if (todo) {
        editingTodoId.value = id
        editTodoText.value = todo.text
      }
    }

    // 編集のキャンセル
    const cancelEdit = () => {
      editingTodoId.value = null
      editTodoText.value = ''
    }

    // 編集の保存
    const saveTodo = () => {
      const todo = todos.value.find(todo => todo.id === editingTodoId.value)
      if (todo && editTodoText.value.trim() !== '') {
        todo.text = editTodoText.value
        editingTodoId.value = null
        editTodoText.value = ''
        saveTodos()
        closeEditModal()
      }
    }

    // コンポーネントがマウントされたときにローカルストレージからタスクを読み込む
    onMounted(loadTodos)

    // `todos`が変わるたびにローカルストレージを更新
    watch(todos, saveTodos, { deep: true })

    return {
      todos,
      newTodoText,
      editingTodoId,
      editTodoText,
      isEditModalVisible,
      addTodo,
      deleteTodo,
      editTodo,
      cancelEdit,
      openEditModal,
      closeEditModal,
      saveTodo,
    }
  },
})
</script>

<template>
  <!-- 入力エリア -->
  <form @submit.prevent="addTodo" class="input">
    <input
      v-model="newTodoText"
      type="text"
      class="input_textArea"
      placeholder="New Task"
    />
    <button type="submit" class="input_button">Add</button>
  </form>

  <!-- Todoリストエリア -->
  <div class="todo">
    <ul class="todo_list">
      <li v-for="todo in todos" :key="todo.id" class="todo_item">
        <div class="todo_itemWrapper">
          <p class="todo_text">{{ todo.text }}</p>
          <div class="todo_buttonWrapper">
            <button @click="openEditModal(todo)" class="todo_editButton">
              Edit
            </button>
            <button @click="deleteTodo(todo.id)" class="todo_deleteButton">
              Del
            </button>
          </div>
        </div>
      </li>
    </ul>
  </div>

  <!-- 編集ポップアップ -->
  <EditModal :isVisible="isEditModalVisible" @close="closeEditModal">
    <h3 class="modal_title">Edit Task</h3>
    <form class="input">
      <input
        v-model="editTodoText"
        type="text"
        placeholder="Edit Task"
        class="input_textArea"
      />
      <button @click="saveTodo" class="input_button -done">Done</button>
    </form>
    <button @click="closeEditModal" class="modal_close">Close</button>
  </EditModal>
</template>

<style lang="scss" scoped>
@use '@/assets/mixin.scss';
.input {
  max-width: 340px;
  display: flex;
  justify-content: center;
  gap: 20px;
  margin: 0 auto;
  &_textArea {
    font-size: 20px;
    border-radius: 30px;
    padding: 10px;
  }
  &_button {
    $this: &;
    color: hsla(160, 100%, 37%, 1);
    background-color: transparent;
    font-weight: 700;
    padding: 10px;
    font-size: 20px;
    border-style: none;
    border-width: 1px;
    border-radius: 20px;
    &:hover {
      cursor: pointer;
      background-color: hsla(160, 100%, 37%, 0.2);
      transition-duration: 0.5s;
    }
    &.-done {
      color: rgb(11, 130, 185);
      &:hover {
        background-color: hsla(210, 70%, 46%, 0.2);
      }
    }
  }
}

.todo {
  margin-top: 20px;
  &_list {
    max-width: 500px;
    margin: 0 auto;
    padding: 0;
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
  &_item {
    border: 1px solid rgba(212, 201, 201, 0.5);
    padding: 16px;
    border-radius: 60px;
  }
  &_text {
    color: white;
    font-size: 22px;
    max-width: 374px;
    font-weight: 500;
    overflow-wrap: anywhere;
  }
  &_itemWrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
    @include mixin.sp {
      flex-direction: column;
      gap: 10px;
    }
  }
  &_buttonWrapper {
    display: flex;
  }
  &_editButton,
  &_deleteButton {
    background-color: transparent;
    font-weight: 700;
    padding: 10px;
    font-size: 20px;
    border-style: none;
    border-width: 1px;
    border-radius: 20px;
    width: fit-content;
    &:hover {
      cursor: pointer;
      transition-duration: 0.5s;
    }
  }
  &_editButton {
    color: rgb(11, 130, 185);
    &:hover {
      background-color: hsla(210, 70%, 46%, 0.2);
      transition-duration: 0.5s;
    }
  }
  &_deleteButton {
    color: rgb(219, 63, 11);
    &:hover {
      background-color: hsla(0, 88%, 45%, 0.2);
      transition-duration: 0.5s;
    }
  }
}

.modal {
  &_title {
    font-size: 30px;
    font-weight: 700;
    color: white;
  }
  &_close {
    color: rgb(231, 230, 230);
    background-color: transparent;
    font-weight: 700;
    padding: 10px;
    font-size: 20px;
    border-style: none;
    border-width: 1px;
    border-radius: 20px;
    width: fit-content;
    &:hover {
      cursor: pointer;
      background-color: hsla(210, 4%, 78%, 0.2);
      transition-duration: 0.5s;
    }
  }
}
</style>
