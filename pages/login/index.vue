<template>
  <div class="container">
    <div class="content">
      <img src="@/assets/img/logo_sales_app.svg" alt />
      <input v-model="user" type="text" placeholder="username" />
      <input v-model="pass" type="text" placeholder="password" />
      <button @click="actionLogin" class="btn btn-primary">Login</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: '',
      pass: ''
    }
  },
  methods: {
    async actionLogin() {
      console.log(this)

      // GET ACCESS TOKEN
      this.$axios.setHeader('Authorization', 'Basic ' + process.env.baseAuth)
      this.$axios.setHeader(
        'Content-Type',
        'application/x-www-form-urlencoded',
        ['post']
      )
      const dataLogin = {
        grant_type: 'password',
        username: this.user,
        password: this.pass
      }
      const querystring = require('querystring')
      const postLogin = querystring.stringify(dataLogin)
      const dataToken = await this.$axios
        .$post('https://betelgeuse.gudangada.com/oauth/token', postLogin)
        .catch(function(e) {
          return e.response
        })

      // GET USER UUID
      this.$axios.setHeader('Accept', 'application/vnd.api+json')
      this.$axios.setHeader('Content-Type', 'application/vnd.api+json')
      this.$axios.setHeader('Authorization', 'Bearer ' + dataToken.access_token)
      const uuid = await this.$axios
        .$get('https://betelgeuse.gudangada.com/jsonapi')
        .catch(function(e) {
          return e.response
        })

      // GET USER DETAIL
      console.log(uuid)
    }
  }
}
</script>

<style scoped>
.container {
  max-width: 1300px;
  padding: 10%;
}

.content {
  margin: auto;
  width: 50%;
}

.content input,
.content img,
.content button {
  display: block;
  margin: auto;
  margin-bottom: 1vh;
}

input[type='text'] {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
}

.btn {
  padding: 14px 24px;
  border: 0 none;
  border-radius: 10px;
  font-weight: 700;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.btn:focus,
.btn:active:focus,
.btn.active:focus {
  outline: 0 none;
}

.btn-primary {
  background: #0099cc;
  color: #ffffff;
}

.btn-primary:hover,
.btn-primary:focus,
.btn-primary:active,
.btn-primary.active,
.open > .dropdown-toggle.btn-primary {
  background: #33a6cc;
}

.btn-primary:active,
.btn-primary.active {
  background: #007299;
  box-shadow: none;
}
</style>
