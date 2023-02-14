<template>      
    <div class="app">
        <h1>Contacts page</h1>
        <my-input
        v-model="searchQuery"
        placeholder="Search.."
        />
        <div class="app__btns">
            <my-button
            @click="showDialog"
            >Create contact
            </my-button>
            <my-select
            v-model="selectedSort"
            :options="sortOptions"
            />
        </div>
        <my-dialog v-model:show="dialogVisible">
            <post-form
        @create="createPost"
        />
        </my-dialog>
        
        <post-list
         :posts="sortedAndSearchedContact"
         @remove="removePost"
         v-if="!isContactsLoading"
         />
         <div v-else>Loading...</div>
    </div>
</template>

<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import axios from "axios";
export default{
    components: {
        PostList, PostForm
    },
    data() {
        return{
            posts: [],
            dialogVisible: false,
            isContactsLoading: false,
            selectedSort: '',
            searchQuery: '',
            sortOptions: [
                {value: 'title', name: 'Sort by name'},
                {value: 'body', name: 'Sort by number'},
            ]
        }
    },
    methods: {
       createPost(post){
          this.posts.push(post);
          this.dialogVisible = false;
        },
        removePost(post){
            this.posts = this.posts.filter(p => p.id !== post.id)
        },
        showDialog(){
            this.dialogVisible = true;
        },
        async fetchContacts(){
            try {
                this.isContactsLoading = true;
                const response = await axios.get('https://63e4e2f7c04baebbcdae55db.mockapi.io/api/v1/contacts');
                this.posts = response.data;
                console.log('response', response)
            } catch (error) {
                console.log('error', error)
            } finally{
                this.isContactsLoading = false;
            }
        }
       
    },
    mounted(){
        this.fetchContacts();
    },
    computed: {
        sortedContacts() {
            return [...this.posts].sort((a,b)=> {
            return a[this.selectedSort]?.localeCompare(b[this.selectedSort])
           })
        },
        sortedAndSearchedContact(){
            return this.sortedContacts.filter(contact => contact.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
        }
    },
    watch:{
    
    }
}
</script>

<style>
    *{
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    .app{
        padding: 20px;
    }
    .app__btns{
        margin: 15px 0;
        display: flex;
        justify-content: space-between;
    }
    

   
</style>