<template>
    <div class="app">
        <h1>Page with posts</h1>
        <div class="app__btns">  
            <my-button @click="showDialog" style="margin:20px 0 20px 0">Create post</my-button>
            <my-select v-model="selectedSort" :options="sortOptions"/>

        </div>
        <!-- <my-button @click="fetchUsers">json</my-button> -->
        <!-- <input type="text" v-model.trim = "modificatorValue" > -->
        <my-dialog v-model:show="dialogValue">
            <post-form @create="createPost" :show="true" />
        </my-dialog>
        <post-list :posts="posts" @remove="removePost" v-if="!isPostLoading" />
        <div v-else>Loading ... </div>
    </div>
</template>
<script>
import PostForm from './components/PostForm.vue'
import PostList from './components/PostList.vue'
import axios from "axios"
export default {
    components: { PostForm, PostList },
    data() {
        return {
            posts: [],
            dialogValue: false,
            // modificatorValue:"",
            isPostLoading: false,
            selectedSort:"",
            sortOptions:[
                {value:"title", name:"By name"},
                {value:"body", name:"By content"},
            ]
        }
    },
    methods: {
        createPost(post) {
            this.posts.push(post);
            this.dialogValue = false
        },
        removePost(post) {
            this.posts = this.posts.filter(p => p.id !== post.id)
        },
        showDialog() {
            this.dialogValue = true
        },
        async fetchUsers() {
            try {
                this.isPostLoading = true
                const response = await axios.get("https://jsonplaceholder.typicode.com/posts?_limit=10")
                this.posts = response.data;

            } catch (e) {
                alert("error")
            } finally {
                this.isPostLoading = false;
            }
        }
    },
    mounted() {
        this.fetchUsers()
    }
}
</script>
<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0
}

.app {
    padding: 20px;
}
.app__btns{
    display: flex;
    justify-content: space-between;
}
</style>
