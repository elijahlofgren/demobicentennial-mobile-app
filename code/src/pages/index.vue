<template>
  <q-page class="flex flex-center">
   
  <ul v-if="markers && markers.length">
    <li v-for="marker of markers">
      <p><strong>{{marker.title}}</strong></p>
      <p>{{marker.County}}</p>
      <a :href="marker.hereMapsUrl">Directions (Here Map)</a>
      <a :href="marker.gmapsUrl">Directions (Google Map)</a>
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
import axios from "axios";

export default {
  name: "PageIndex",
  data() {
    return {
      markers: [],
      errors: []
    };
  },

  // Fetches markers when the component is created.
  created() {
    // temporarily use static copy of some sample data until azure account is restored
    axios
      .get(
        `https://rawgit.com/HackMGM/demobicentennial-app/master/sample-json-data/top-level-gps.json`
      )
      // disabled azure server reference
      // axios.get(`http://13.82.106.207/?q=mobileapi/markersjson.json`)
      .then(response => {
        var preppedData = response.data;

        // URL like
        // https://www.google.com/maps/?q=32.37685,-86.30078333 -->
        var gmapUrlPrefix = "https://www.google.com/maps/?q=";
        var hereMapsUrlPrefix = "https://wego.here.com/search/";
        for (var i = 0; i < preppedData.length; i++) {
          var marker = preppedData[i];
          if (marker.Coordinates && marker.Coordinates.length > 1) {
            preppedData[i].gmapsUrl =
              gmapUrlPrefix +
              marker.Coordinates[0] +
              "," +
              marker.Coordinates[1];

              preppedData[i].hereMapsUrl =
              hereMapsUrlPrefix +
              marker.Coordinates[0] +
              "," +
              marker.Coordinates[1];
          }
        }

        console.log(JSON.stringify(preppedData));

        // JSON responses are automatically parsed.
        this.markers = preppedData;
      })
      .catch(e => {
        this.errors.push(e);
      });

    // async / await version (created() becomes async created())
    //
    // try {
    //   const response = await axios.get(`http://jsonplaceholder.typicode.com/posts`)
    //   this.posts = response.data
    // } catch (e) {
    //   this.errors.push(e)
    // }
  }
};
</script>
