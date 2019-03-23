<template>
  <div class="wrap">
    <div class="search">
      <input type="text" class="searchTerm" placeholder="What are you looking for?" v-model="addressString">
      <button type="submit" class="searchButton" @click="loadOutlet">
        <i class="fa fa-search"></i>
      </button>
      <div></div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import config from '../../config';
  const API_URL = config.build.api_url;
    export default {
      name: "OutletFinder.vue",
      data() {
        return {
          addressString: '',
        };
      },
      methods: {
        getOutlet(addressString) {
          const url = `${API_URL}/outlet`;
          const options = {
            params: {
              address : addressString
            }
          }
          return axios.get(url, options)
        },
        async loadOutlet () {
          try {
          const data = await this.getOutlet(this.addressString);
            this.$toasted.show(data.data.data.name, {type: "success"});

          } catch (e) {
            if (e.response.data.error.code === 1001) {
                this.$toasted.show(e.response.data.error.message.address.msg, {type: "error"});
            } else {
              this.$toasted.show(e.response.data.error.message, {type: "error"});
            }
          }
        }
      }
    }
</script>

<style scoped>
  @import url(https://fonts.googleapis.com/css?family=Open+Sans);

  body{
    background: #f2f2f2;
    font-family: 'Open Sans', sans-serif;
  }

  .search {
    width: 100%;
    position: relative
  }

  .searchTerm {
    float: left;
    width: 100%;
    border: 3px solid #00B4CC;
    padding: 5px;
    height: 20px;
    border-radius: 5px;
    outline: none;
    color: #9DBFAF;
  }

  .searchTerm:focus{
    color: #00B4CC;
  }

  .searchButton {
    position: absolute;
    right: -50px;
    width: 40px;
    height: 36px;
    border: 1px solid #00B4CC;
    background: #00B4CC;
    text-align: center;
    color: #fff;
    border-radius: 5px;
    cursor: pointer;
    font-size: 20px;
  }

  /*Resize the wrap to see the search bar change!*/
  .wrap{
    width: 30%;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
</style>
