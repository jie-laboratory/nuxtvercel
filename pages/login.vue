<template>
  <div class="mt-64 p-10 grid bg-green-600 flex justify-center">
    <input v-model="email" type="text" placeholder="Email" />
    <br />
    <input v-model="password" type="text" placeholder="Password" />
    <br />
    <button class="bg-black text-white" @click="login">Login</button>
    <button class="bg-black text-white" @click="register">Register</button>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: 'test2@ckcm.edu.ph',
      password: 'password',
      message: '',
    }
  },
  methods: {
    async login() {
      const vm = this
      vm.message = 'loggin in...'
      await this.$auth
        .loginWith('laravelSanctum', {
          data: {
            email: vm.email,
            password: vm.password,
          },
        })
        .then((response) => {
          vm.message = ''
          vm.$router.push('/')
        })
        .catch((error) => {
          vm.message = error.response.data.message
        })
    },
    async register() {
      const vm = this
      vm.message = 'loading...'
      await this.$axios
        .$post('/jieBackpain/api/register', {
          email: vm.email,
          password: vm.password,
          name: 'test',
        })
        .then((response) => {
          vm.login()
        })
        .catch((error) => {
          vm.message = error.response.data.message
        })
    },
  },
}
</script>

<style lang="scss" scoped></style>
