<template>
  <v-app>
    <v-main>
      <PostsList />
    </v-main>
  </v-app>
</template>

<script>
import PostsList from "@/components/PostsList";

export default {
  name: 'App',

  components: {
    PostsList,
  },

  data: () => ({
    //
  }),
};
</script>
