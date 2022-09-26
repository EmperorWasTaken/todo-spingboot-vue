<template>
    <el-form @submit.prevent :inline="true" :model="formInput" class="todo-form-input">
        <el-form-item label="Title" prop="title">
            <el-input v-model="formInput.title" placeholder="Enter Todo"></el-input>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="onSubmit()">Add</el-button>
        </el-form-item>
    </el-form>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";

@Options({})
export default class TodoForm extends Vue {
    formInput = { title: "", completed: false };

    onSubmit(){
        console.log("Todo", this.formInput)
    }
}
</script>