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

    <b-field label="Email">
            <b-input type="email"
                v-model="email">
            </b-input>
        </b-field>

    <br/>
    <button class="button is-danger" @click="handleSignup">Sign Up</button>
  </div>
</template>

<script>

export default {
  name: "Signup",
  data: function (){
    return{
  username:"",
  password:"",
  email:""
    };
  },
methods: {
  handleSignup: function () {
    fetch("http://127.0.0.1:8000/auth/users/register/", {
      method: "post",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify({
        username: this.username,
        password: this.password,
        email: this.email
      }),
    })
        .then(response => response.json())
        .then(data => {
          console.log(data)
          this.$emit('loggedIn', data);
        });
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