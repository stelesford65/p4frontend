<template>
  <div class="main">
        <b-field label="Blog Title" type="is-danger" message="Enter a title here">
            <b-input type="text" v-model="title" maxlength="30"></b-input>
        </b-field>
    <b-field label="Description" type="is-danger" message="Enter a description here">
            <b-input type="text" v-model="description" maxlength="500"></b-input>
        </b-field>
      <b-button type="is-danger is-light" @click="newBlogEntry">New Blog Entry </b-button><br/><br/>
 <b-field label="Image-URL" type="is-danger" message="Add an image here">
            <b-input type="text" v-model="image" maxlength="500"></b-input>
        </b-field>
     <b-button type="is-danger is-light" @click="newPicture">New Picture </b-button><br/><br/>

      <b-field label="Edit Your Title" type="is-danger" message="Edit Title Here">
            <b-input type="text" v-model="edittitle" maxlength="30"></b-input>
        </b-field>
    <b-field label="Edit Your Description" type="is-danger" message="Edit Description Here">
            <b-input type="text" v-model="editdescription" maxlength="500"></b-input>
        </b-field>
    <b-button type="is-danger is-light" @click="editBlogEntry" v-bind:id="editid">Edit Blog Entry </b-button><br/><br/>
    <b-field label="Image-URL" type="is-danger" message="Add an image here">
            <b-input type="text" v-model="editimage" maxlength="500"></b-input>
        </b-field>
    <b-button type="is-danger is-light" @click="editPicture" v-bind:id="editid">Edit Picture</b-button><br/><br/>
    <ul>
      <li v-for="blog_entry of blog_entrys.results" v-bind:key="blog_entry.id">
        <b-message title="Blog" aria-close-label="Close message">
            {{blog_entry.title}}
            {{blog_entry.description}}
          <button v-bind:id="blog_entry.id" class="button is-success is-outlined" @click = deleteBlogEntry>Delete</button>
          <button v-bind:id="blog_entry.id" class="button is-success is-outlined" @click="()=>{editSelect(blog_entry.id, blog_entry.title, blog_entry.description)}">Edit</button>
        </b-message>
      </li>
       <li v-for="picture of pictures.results" v-bind:key="picture.id">
        <section>
            {{picture.image}}
          <img v-bind:src="picture.image" height="300" width="300">
          <button v-bind:id="picture.id" class="button is-success is-outlined" @click = deletePicture>Delete</button>
          <button v-bind:id="picture.id" class="button is-success is-outlined" @click="()=>{editSelectPicture(picture.id, picture.image)}">Edit</button>
        </section>
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
      pictures: [],
      title:"",
      description:"",
      image:"",
      edittitle:"",
      editdescription:"",
      editimage:"",
      editid: null
    }
  },
  created: function(){
this.getBlogEntry()
    this.getPicture()
  },

  methods: {
    newBlogEntry: function(){
      const {tokens} = this.$route.query
      console.log(tokens)
      fetch("http://127.0.0.1:8000/api/blog_entry/",{
      method: 'post',
      headers:{
        authorization: `JWT ${tokens.token}`,
        'Content-Type':'application/json',
      },
        body: JSON.stringify({title: this.title, description: this.description})
    })
      .then(() =>{
        this.getBlogEntry()
      })
    },
    getBlogEntry: function (){
      const {tokens} = this.$route.query
    fetch("http://127.0.0.1:8000/api/blog_entry/",{
      method: 'get',
      headers:{
        authorization: `JWT ${tokens.token}`
      }
    })
    .then(response => response.json())
    .then(data => {
      this.blog_entrys = data
    })
    },
    deleteBlogEntry: function (event){
      const {tokens} = this.$route.query
      const id = event.target.id

      fetch(`http://127.0.0.1:8000/api/blog_entry/${id}/`,{
      method: 'delete',
      headers:{
        authorization: `JWT ${tokens.token}`
      }
    })
      .then(()=>{
        this.getBlogEntry()
      })
    },
    editSelect: function(id, title, desc){
      this.editid = id
      this.edittitle = title
      this.editdescription = desc
    },
    editBlogEntry: function (){
      const {tokens} = this.$route.query
      const id = this.editid

      fetch(`http://127.0.0.1:8000/api/blog_entry/${id}/`,{
      method: 'patch',
      headers:{
        authorization: `JWT ${tokens.token}`,
        "Content-Type":"application/json"
      },
        body:JSON.stringify({title: this.title, description: this.description})
    })
      .then(()=>{
        this.getBlogEntry()
      })
    },
     newPicture: function(){
      const {tokens} = this.$route.query
      console.log(tokens)
      fetch("http://127.0.0.1:8000/api/pictures/",{
      method: 'post',
      headers:{
        authorization: `JWT ${tokens.token}`,
        'Content-Type':'application/json',
      },
        body: JSON.stringify({image: this.image})
    })
      .then(() =>{
        this.getPicture()
      })
    },
getPicture: function (){
      const {tokens} = this.$route.query
    fetch("http://127.0.0.1:8000/api/pictures/",{
      method: 'get',
      headers:{
        authorization: `JWT ${tokens.token}`
      }
    })
    .then(response => response.json())
    .then(data => {
      this.pictures = data
    })
    },
    deletePicture: function (event){
      const {tokens} = this.$route.query
      const id = event.target.id

      fetch(`http://127.0.0.1:8000/api/pictures/${id}/`,{
      method: 'delete',
      headers:{
        authorization: `JWT ${tokens.token}`
      }
    })
      .then(()=>{
        this.getPicture()
      })
    },
    editSelectPicture: function(id, image){
      this.editid = id
      this.editimage = image
    },
    editPicture: function (){
      const {tokens} = this.$route.query
      const id = this.editid

      fetch(`http://127.0.0.1:8000/api/pictures/${id}/`,{
      method: 'patch',
      headers:{
        authorization: `JWT ${tokens.token}`,
        "Content-Type":"application/json"
      },
        body:JSON.stringify({image: this.image})
    })
      .then(()=>{
        this.getPicture()
      })
    },
  },
};
</script>


<style>
div.main{
  width: 70%;
  min-width: 300px;
  margin: 10px auto;
}
</style>