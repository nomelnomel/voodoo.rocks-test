<template>
  <div>
    <div class="search">
      <v-form>
        <v-row class="justify-center">
          <v-col
            cols="6"
            md="3"
          >
            <v-text-field
              v-model="searchString"
              label="search by author"
              prepend-inner-icon="mdi-magnify"
            />
          </v-col>
        </v-row>
      </v-form>
    </div>
    <div v-if="filteredPosts.length > 0">
      <v-container class="grey lighten-5">
        <v-row align="stretch">
          <template v-for="{title, body, author, id} in filteredPosts">
            <v-col
              :key="id"
              cols="12"
              sm="6"
            >
              <v-card class="fill-height d-flex flex-column justify-space-between">
                <v-card-title class="title d-flex align-start">
                  {{ title }}
                </v-card-title>
                <v-card-text>
                  {{ body }}
                </v-card-text>
                <v-card-subtitle class="align-self-end">
                  {{ author }}
                </v-card-subtitle>
              </v-card>
            </v-col>
          </template>
        </v-row>
      </v-container>
    </div>
    <div v-else>
      <p class="text-center">
        No posts by this author
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "PostsList",
  data() {
    return {
      posts: [],
      searchString: ''
    }
  },
  computed: {
    filteredPosts() {
      return this.posts.filter(({author}) => author.toLowerCase().includes(this.searchString.toLowerCase()))
    }
  },
  async created(){
    try {
      const [posts, users] = await Promise.all([
        this.axios.get('https://jsonplaceholder.typicode.com/posts'),
        this.axios.get('https://jsonplaceholder.typicode.com/users')
      ])

      this.posts = posts.data.map(post => {
        const author = users.data.find(({id}) => id === post.userId).name
        return {
          ...post,
          author
        }
      })
    }catch (e) {
      console.log(e)
    }
  },

}
</script>

<style scoped>
</style>