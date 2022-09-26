<template>
    <h1>TodoList</h1>
    <todo-form />
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import TodoForm from "./TodoForm.vue";

@Options({
    components: {
        TodoForm,
    }
})
export default class TodoList extends Vue {
    
}
</script>