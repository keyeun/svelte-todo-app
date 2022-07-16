<script>
  import  { todos, saveStorage } from '~/store'
  export let todo

  let isEditmode = false
  let title = ''

  function onEditMode() {
    isEditmode = true
    title = todo.title
  }

  function offEditMode() {
    isEditmode = false
  }

  function updateTodo() {
    todo.title = title
    saveStorage()
    offEditMode()
  }

  function deleteTodo() {
    $todos = $todos.filter(t => t.id !== todo.id)
    saveStorage()
  }

</script>

<div class="todo">
  {#if isEditmode}
    <div class="edit-mode">
      <input 
        bind:value = {title} 
        type="text" 
        class="form-control"
        on:keyup={e => {
          if (e.key === 'Enter') updateTodo()
        }}>
        <button 
        class = "btn btn-primary"
        on:click={updateTodo}>
        OK
      </button>
      <button 
        class = "btn btn-secondary"
        on:click={offEditMode}>
        Cancel
      </button>
    </div>
  {:else}
    <div class="normal-mode">
      <div class="title">
        {todo.title}
      </div>
      <button 
        class="btn btn-secondary"
        on:click={onEditMode}>
        Edit
      </button>
      <button 
        class="btn btn-danger"
        on:click={deleteTodo}>
        Delete
      </button>
    </div>
  {/if}
</div>

<style lang="scss">
  .todo {
    padding: 10px 14px;
    border-radius: 6px;
    &:hover {
      background-color: $gray-100;
    }
    .edit-mode,
    .normal-mode {
      display: flex;
    }
    .title{
      flex-grow: 1;
      display: flex;
      align-items: center;
      font-size: 18px;
    }
    .btn {
      flex-shrink: 0;
      margin-left: 10px;
    }
  }
</style>