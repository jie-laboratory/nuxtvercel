<template>
  <div class="">
    <button @click="register">Login</button>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: '',
    }
  },
  methods: {
    async login() {
      const vm = this
      await this.$auth
        .loginWith('laravelSanctum', {
          data: {
            email: 'test@ckcm.com',
            password: 'password',
          },
        })
        .then((response) => {
          vm.$router.push('/')
        })

      console.log(this.$auth)
    },
    async register() {
      const vm = this
      await this.$axios
        .$post('localhost:8000/api/register', {
          email: 'test@ckcm.com',
          password: 'password',
          name: 'test',
        })
        .then((response) => {
          vm.$router.push('/registered')
        })
        .catch((message) => {
          vm.message = message
        })
    },
  },
}
</script>

<style lang="scss" scoped></style>
