<template>
    <div v-if="(posts.length>0)">
        <h4>Users list</h4>
        <post-item  v-for="post in posts" :post="post" :key="post.id" @remove="$emit('remove', post)"/>
    </div>
    <div v-else>
        <h1 class="red">No posts</h1>
    </div>
</template>
<script>
import PostItem from "@/components/PostItem.vue"

export default {
    components:{
        PostItem
    },
    props:{
        posts:{
            type:Array,
            required:true
        }
    }
}
</script>
<style scoped>
    .red{
        color:red
    }
</style>
