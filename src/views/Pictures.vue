<template>
  <div>
    <button @click="callApi">Update</button>
    <h1>{{apiMessage}}</h1>
    <img v-if="pictureUrl" :src="pictureUrl[0]" alt="nothing">
    <img v-if="pictureUrl" :src="pictureUrl[1]" alt="nothing">
    <img v-if="pictureUrl" :src="pictureUrl[2]" alt="nothing">

  </div>
</template>
<script lang="ts">

import axios from "axios";

//type apiData = {"success": Boolean, "files": [{"id": String, "filename": String }]}

export default {
  name: "pictures",
  components: {
  },
  data() {
    return {
      apiMessage: {"success": Boolean, "files": {"id": String, "filename": String }},
      pictureUrl: Array
    };
  },
  methods: {
    async callApi() {
      let array = [];
      // Use Axios to make a call to the API
   //  type apiData = {"success": Boolean, "files": [{"id": String, "filename": String }]}
      await axios.get("http://localhost:3001/images").then((fetchData)=>{
       fetchData.data.files.forEach(function (img){
          array.push(`http://localhost:3001/image/${img.filename}?tn=${true}`);
       })
        this.pictureUrl = array;
        //this.apiMessage = fetchData.data
      })
    },
  }
};
</script>
<style lang="scss" scoped>
.next-steps {
  .fa-link {
    margin-right: 5px;
  }
}
</style>