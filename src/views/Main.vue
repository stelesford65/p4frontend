<template>
  <div class="main">
        <b-field label="New blog entry" type="is-danger" message="Enter a blog entry here">
            <b-input type="text" v-model="message" maxlength="30"></b-input>
        </b-field>
      <b-button type="is-danger is-light" @click="newBlogEntry">New Blog Entry </b-button><br/><br/>

      <b-field label="New blog entry" type="is-danger" message="Edit a blog entry here">
            <b-input type="text" v-model="editmessage" maxlength="30"></b-input>
        </b-field>
    <b-button type="is-danger is-light" @click="editBlogEntry" v-bind:id="editid">Edit Blog Entry </b-button><br/><br/>
    <ul>
      <li v-for="blog_entry of blog_entrys" v-bind:key="blog_entry.id">
        <b-message title="Default" aria-close-label="Close message">
            {{blog_entry.message}}
          <button v-bind:id="blog_entry.id" class="button is-success is-outlined" @click = deleteBlogEntry>Delete</button>
          <button v-bind:id="blog_entry.id" class="button is-success is-outlined" @click="()=>{editSelect(blog_entry.id, blog_entry.message)}">Edit</button>
        </b-message>
      </li>
    </ul>
  </div>
</template>

<script>


export default {
  name: 'Main',
  data: function (){
    return{
      blog_entrys: [],
      message:"",
      editmessage:"",
      editid: null
    }
  },
  created: function(){
this.getBlogEntry()
  },
  methods: {
    newBlogEntry: function(){
      const {tokens, URL} = this.$route.query
      fetch(`${URL}/api/blog_entry/`,{
      method: 'post',
      headers:{
        authorization: `JWT ${tokens}`,
        'Content-Type':'application/json',
      },
        body: JSON.stringify({message: this.message})
    })
      .then(() =>{
        this.getBlogEntry()
      })
    },
    getBlogEntry: function (){
      const {tokens, URL} = this.$route.query
    fetch(`${URL}/api/blog_entry/`,{
      method: 'get',
      headers:{
        authorization: `JWT ${tokens}`
      }
    })
    .then(response => response.json())
    .then(data => {
      this.notes = data
    })
    },
    deleteBlogEntry: function (event){
      const {tokens, URL} = this.$route.query
      const id = event.target.id

      fetch(`${URL}/api/blog_entry/${id}/`,{
      method: 'delete',
      headers:{
        authorization: `JWT ${tokens}`
      }
    })
      .then(()=>{
        this.getBlogEntry()
      })
    },
    editSelect: function(id, content){
      this.editid = id
      this.editmessage = content
    },
    editBlogEntry: function (){
      const {tokens, URL} = this.$route.query
      const id = this.editid

      fetch(`${URL}/api/blog_entry/${id}/`,{
      method: 'put',
      headers:{
        authorization: `JWT ${tokens}`,
        "Content-Type":"application/json"
      },
        body:JSON.stringify({message: this.editmessage})
    })
      .then(()=>{
        this.getBlogEntry()
      })
    },

  }
}
</script>

<style>
div.main{
  width: 70%;
  min-width: 300px;
  margin: 10px auto;
}
</style>