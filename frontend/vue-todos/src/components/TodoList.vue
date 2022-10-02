<template>
  <el-row>
    <el-col :span="12" :offset="7" style="width: 100%">
      <h1>TodoList</h1>
      <todo-form @send-message="createTodo" />
      <el-table :data="todos">
        <el-table-column
          prop="title"
          label="Title"
          width="350"
        ></el-table-column>
        <el-table-column fixed="right" label="Operations" width="200"
          ><template #default="scope">
            <el-space wrap>
              <el-switch
                v-model="scope.row.completed"
                @click="updateTodo(scope.row)"
              ></el-switch>
            </el-space>
            <el-popconfirm
              confirm-button-text="Yes"
              cancel-button-text="No"
              icon="el-icon-info"
              icon-color="red"
              size="default"
              title="Are you sure you want to delete this?"
              @confirm="handleDelete(scope.row)"
              @cancel="cancelDelete"
            >
                <template #reference>
                    <el-button size="default" type="danger">Delete</el-button>
                </template>
            </el-popconfirm>
          </template>
        </el-table-column>
      </el-table>
    </el-col>
  </el-row>
</template>

<script lang="ts">
import { ElMessage } from "element-plus";
import { Options, Vue } from "vue-class-component";
import TodoForm from "./TodoForm.vue";

interface Todo {
  id: number;
  title: string;
  completed: boolean;
}

@Options({
  components: {
    TodoForm,
  },
})
export default class TodoList extends Vue {
  todos = [];

  async mounted() {
    await this.loadTodos();
  }

  async loadTodos() {
    const response = await this.axios.get("todos");
    this.todos = response.data;
  }
  async createTodo(todo: Todo) {
    console.log("Todo", todo);
    await this.axios.post("todos", {
      title: todo.title,
      completed: todo.completed,
    });
    ElMessage({
      message: "Todo Created",
      type: "success",
    });
    await this.loadTodos();
  }

  async updateTodo(todo: Todo) {
    console.log("Todo", todo);
    await this.axios.put("todos/${todo.id}", {
      id: todo.id,
      title: todo.title,
      completed: todo.completed,
    });
    ElMessage({
      message: "Todo updated",
      type: "success",
    });
    await this.loadTodos();
  }

  async handleDelete(todo: Todo) {
    await this.axios.delete("todos/${todo.id}");
    ElMessage({
      message: "Todo deleted",
      type: "success",
    });
    await this.loadTodos();
  }

  cancelDelete() {
    console.log("Cancelled deletion");
  }
}
</script>
