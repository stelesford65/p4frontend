<template>


    <b-navbar>
        <template slot="brand">
            <b-navbar-item tag="router-link" :to="{ path: '/' }">
              <img class ="img1" src = "https://res.cloudinary.com/dju2f4kdt/image/upload/v1600768184/students_alyycy.png">
            </b-navbar-item>
        </template>


        <template slot="end">
            <b-navbar-item tag="div">
                <div class="buttons">

                        <router-link :to= "{ name: 'Signup', query: {URL: this.URL}}" v-bind:URL="URL"
                     v-if="!loggedIn"><button class="button is-link">Sign Up</button></router-link>

                     <router-link :to= "{ name: 'Login', query: {URL: this.URL}}" v-bind:URL="URL"
                     v-if="!loggedIn"><button class="button is-link">Log In</button></router-link>
                     <button v-if="loggedIn" class="button is-link" @click="logout">Logout</button>
                </div>
            </b-navbar-item>
        </template>
    </b-navbar>
</template>

<script>

export default {
  name:'Header',
  props:['URL','loggedIn'],
  methods: {
    logout: function(){
      this.$emit('logout')
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Barriecito&display=swap');
nav {
  width: 80%;
  margin: 10px auto;
}

span.logo {
  font-family: Barriecito;
  font-size: 2em;
}
.img1{
    width: 50px;
    height: 50px;
        position: center;
    }
</style>