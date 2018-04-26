<template>
  <q-page class="flex flex-center">
   
  <ul v-if="posts && posts.length">
    <li v-for="post of posts">
      <p><strong>{{post.title}}</strong></p>
      <p>{{post.County}}</p>
    </li>
  </ul>

  <ul v-if="errors && errors.length">
    <li v-for="error of errors">
      {{error.message}}
    </li>
  </ul>
   test
    <img src="~assets/quasar-logo-full.svg">
  </q-page>

</template>

<script>
import axios from 'axios';

export default {
  name: 'PageIndex',
  data() {
    return {
      posts: [],
      errors: []
    }
  },

  // Fetches posts when the component is created.
  created() {
    // temporarily use static copy of some sample data until azure account is restored
    axios.get(`https://rawgit.com/HackMGM/demobicentennial-app/master/sample-json-data/top-level-gps.json`)
    // disabled azure server reference
    // axios.get(`http://13.82.106.207/?q=mobileapi/markersjson.json`)
    .then(response => {
      // JSON responses are automatically parsed.
      this.posts = response.data
    })
    .catch(e => {
      this.errors.push(e)
    })

    // async / await version (created() becomes async created())
    //
    // try {
    //   const response = await axios.get(`http://jsonplaceholder.typicode.com/posts`)
    //   this.posts = response.data
    // } catch (e) {
    //   this.errors.push(e)
    // }
  }
}
</script>
