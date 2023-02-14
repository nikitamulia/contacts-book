<template>
    <div v-if="posts.length > 0">
       <h3> Posts list</h3>
       <transition-group name="contact-list">
            <post-item 
            v-for="post in posts"
            :post="post"
            :key="post.id"
            @remove="$emit('remove', post)"
            />
       </transition-group>
    </div>
    <h2 v-else>
        No contacts
    </h2>
           
  
</template>

<script>
import PostItem from "@/components/PostItem.vue"
    export default {
        components: {
            PostItem,
        },
        props: {
            posts: {
                type: Array,
                required: true,
            }
        }
    }
</script>

<style  scoped>
.contact-list{
    display: inline-block;
    margin-right: 10px;
}
.contact-list-enter-active,
.contact-list-leave-active {
  transition: all 0.5s ease;
}
.contact-list-enter-from,
.contact-list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
.contact-list-move{
    transition: transform 0.5s ease;
}
</style>