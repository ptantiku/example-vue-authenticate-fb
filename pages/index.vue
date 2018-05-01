<template>
  <section class="container">
    <div>
      <h1 class="title">
        Test-project
      </h1>
      <button @click="authenticate('facebook')">auth Facebook</button>
    </div>
    <div>
      <h2>Result</h2>
      <textarea v-model="token" cols="50" rows="5" />
      <textarea v-model="profile" cols="50" rows="5" />
    </div>  
  </section>
</template>

<script>
export default {
  components: {
  },
  data: () => {
    return {
      token: '',
      profile: '',
    }
  },
  methods: {
    authenticate: function (provider) {
      const this_ = this
      this.$auth.authenticate(provider).then(function () {
        let token = this_.$auth.getToken()
        this_.token = token
        alert(`login success with token ${token}`)
        if (provider === 'facebook') {
          this_.$http.get('https://graph.facebook.com/v3.0/me?fields=id,name,email', {
            params: { access_token: token }
          }).then(function (response) {
            this_.profile = JSON.stringify(response)
          })
        }
      })
    }
  }
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}
textarea {
  display: block;
}
</style>
