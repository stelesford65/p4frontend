<template>
  <div class="login">
     <b-field label="Username"
            type="is-success">
            <b-input v-model="username"></b-input>
        </b-field>

        <b-field label="Password">
            <b-input type="password"
                v-model="password"
                password-reveal>
            </b-input>
        </b-field>

    <br/>
    <button class="button is-danger" @click="handleLogin">Log In</button>
  </div>
</template>

<script>

export default {
  name: "Login",
  data: function (){
    return{
  username:"",
  password:"",
    };
  },
methods: {
   handleLogin: function() {
      fetch("https://stp4backend.herokuapp.com/auth/users/login/",  {
        method: 'post',
        headers: {
          "Content-Type": "application/json"
        },
        body:JSON.stringify({
          username: this.username,
          password: this.password
        }),
      })
      .then(response => {
        if (response.status == 400) {
          response.json()
        } else {
          return response.json()
        }
      })
      .then(data => {
        console.log('data', data)
        if(data){
          this.$emit('loggedIn', data)
        } else {
          alert('Incorrect Login')
        }
      })
    },
},
};
</script>

<style>
.login{
  width: 70%;
  margin: 10px auto;
}
</style>