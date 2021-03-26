

<template>
  <div>
    <button @click="callApi">Update</button>
    <button @click="getImages">images</button>

    <h1>{{apiMessage}}</h1>
    <img    :src="`data:image/png;base64,${picture}`" alt="nothing">
    <div>{{picture}}</div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "pictures",
  components: {
  },
  data() {
    return {
      apiMessage: {"success": Boolean, "files": [{"id": String, "filename": String }]},
      picture: {}
    };
  },
  methods: {
    async callApi() {
      // Get the access token from the auth wrapper
      const token = await this.$auth.getTokenSilently();

      // Use Axios to make a call to the API
      const { data } = await axios.get("http://localhost:3001/images", {
        headers: {
          Authorization: `Bearer ${token}`    // send the access token through the 'Authorization' header
        }
      })
      this.apiMessage = data;



    },
    async getImages() {
      const token = await this.$auth.getTokenSilently();

      const { data } = await axios.get("http://localhost:3001/image/"+this.apiMessage.files[0].filename,{
        responseType: 'arraybuffer',
        headers: {
          Authorization: `Bearer ${token}`    // send the access token through the 'Authorization' header
        }
      })

      this.picture = Buffer.from(data, "binary").toString('base64');
    }

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
