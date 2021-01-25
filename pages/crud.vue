<template>
  <div class="flex flex-col">
    <div class="text-center bg-red-900 text-white">
      <h3>My Post</h3>
    </div>
    <div class="flex mt-10 gap-4 flex-col items-center">
      <div class="flex-auto">
        <input
          v-model="title"
          class="bg-green-100"
          placeholder="Title"
          type="text"
        />
      </div>
      <div class="flex-auto">
        <textarea
          v-model="body"
          class="bg-green-100"
          placeholder="Body"
          type="text"
        />
      </div>
      <div class="flex-auto">
        <button type="submit" @click="submitNOw">Submit</button>
      </div>
    </div>

    <div class="flex mt-10 gap-4 flex-col items-center">
      <table class="table-auto bg-green-900 text-white">
        <thead>
          <tr>
            <th>Index</th>
            <th>Title</th>
            <th>Body</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(data, index) in postLists" :key="data.index">
            <td>{{ data.index }}</td>
            <td>{{ data.title }}</td>
            <td>{{ data.body }}</td>
            <td><button @click="deleteTodo(index)">Delete</button></td>
          </tr>
        </tbody>
      </table>
    </div>
    {{ postLists }}
    <span v-if="loading"> Loading </span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: '',
      body: '',
      postLists: [],
      loading: false,
    }
  },
  created() {
    this.post_GET()
  },
  methods: {
    deleteTodo(index) {
      alert(index)
    },
    submitNOw() {},
    async post_GET() {
      const vm = this
      this.loading = true
      this.postList = await this.$axios
        .$get('/jieBackpain/posts')
        .then((response) => {
          vm.postLists = response
          vm.loading = false
        })
        .catch((error) => {
          alert(error)
        })
    },
  },
}
</script>

<style lang="scss" scoped></style>
