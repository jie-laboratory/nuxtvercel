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
        <button @click="post_STORE">Submit</button>

        <br />
        <br />
        <br />

        <button @click="post_UPDATE">Update</button>
      </div>
    </div>

    <div class="flex mt-10 gap-4 flex-col items-center">
      <div class="text-white">
        <button
          v-if="paginateLinks.first !== null"
          class="bg-green-900"
          @click="paginateLinksButton('first')"
        >
          First
        </button>
        <button
          v-if="paginateLinks.prev !== null"
          class="bg-red-900"
          @click="paginateLinksButton('prev')"
        >
          Previous
        </button>
        <button
          v-if="paginateLinks.next !== null"
          class="bg-yellow-900"
          @click="paginateLinksButton('next')"
        >
          Next
        </button>
        <button
          v-if="paginateLinks.last !== null"
          class="bg-blue-900"
          @click="paginateLinksButton('last')"
        >
          Last
        </button>
      </div>
      <table class="table-auto bg-green-900 text-white">
        <thead>
          <tr>
            <th>Index</th>
            <th>Title</th>
            <th>Body</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(data, index) in postLists" :key="index">
            <td>{{ data.id }}</td>
            <td>{{ data.title }}</td>
            <td>{{ data.body }}</td>
            <td><button @click="viewer(data.id)">View</button></td>
            <td><button @click="updater(data)">Update</button></td>
            <td><button @click="post_DELETE(data.id)">Delete</button></td>
          </tr>
        </tbody>
      </table>
    </div>
    <span v-if="loading"> Loading </span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: '',
      body: '',
      id: '',
      postLists: [],
      loading: false,
      paginateLinks: [],
    }
  },
  created() {
    this.post_GET()
  },
  methods: {
    paginateLinksButton(param) {
      const domain = new URL(this.paginateLinks[param])
      const query = domain.search
      this.$nextTick(() => {
        this.$router.push(`/crud${query}`)
        this.post_GET()
      })
    },
    viewer(id) {
      this.$router.push(`/crud/${id}`)
    },
    deleteTodo(index) {
      alert(index)
    },
    updater(data) {
      this.title = data.title
      this.body = data.body
      this.id = data.id
    },
    async post_UPDATE() {
      const vm = this
      this.loading = true
      await this.$axios
        .$patch(`/jieBackpain/posts/${vm.id}`, {
          title: vm.title,
          body: vm.body,
        })
        .then((response) => {
          vm.post_GET()
        })
        .catch((error) => {
          alert(error)
        })
    },
    async post_STORE() {
      const vm = this
      this.loading = true
      await this.$axios
        .$post('/jieBackpain/posts', {
          title: vm.title,
          body: vm.body,
        })
        .then((response) => {
          vm.post_GET()
        })
        .catch((error) => {
          alert(error.response.data.message)
        })
    },
    async post_DELETE(id) {
      const vm = this
      this.loading = true
      await this.$axios
        .$delete(`/jieBackpain/posts/${id}`)
        .then((response) => {
          vm.loading = false
          vm.post_GET()
        })
        .catch((error) => {
          alert(error)
        })
    },
    async post_GET() {
      const vm = this
      this.loading = true
      await this.$axios
        .$get(`/jieBackpain/posts`, { params: this.$route.query })
        .then((response) => {
          vm.loading = false
          console.log(vm.$route.query)
          vm.paginateLinks = response.links
          vm.postLists = response.data
        })
        .catch((error) => {
          alert(error)
        })
    },
  },
}
</script>

<style lang="scss" scoped></style>
