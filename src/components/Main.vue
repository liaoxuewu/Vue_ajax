<template>
    <div>
      <!--初始化状态-->
      <h2 v-show="initView">Enter name to search</h2>
      <!--加载中-->
      <h2 v-show="loading">Loading...</h2>
      <!--发送请求失败-->
      <h2 v-show="errorMsg">{{errorMsg}}</h2>
      <!--发送请求成功-->
      <div class="row">
        <div class="card" v-for="user in users">
          <a :href="user.userUrl" target="_blank">
            <img :src="user.avatar" style='width: 100px'/>
          </a>
          <p class="card-text">{{user.username}}</p>
        </div>
      </div>
    </div>
</template>
<script>
  import axios from 'axios';

  export default {
    props:['searchName'],
    data () {
      return {
        initView : true,
        loading : false,
        errorMsg : null,
        users: []
      }
    },
    watch: {
      searchName : function (value) {
        //更新状态
        this.initView = false;
        this.loading = true;
        this.users = [];
        this.errorMsg = null;

        //发送ajax请求
        const url = `https://api.github.com/search/users?q=${value}`;
        axios.get(url)
          .then(response => {
            //更新状态（请求成功）
            console.log(response.data);
            this.loading = false;
            this.users = response.data.items.map(item => {
              return {
                userUrl:item.html_url,
                avatar:item.avatar_url,
                username:item.login
              }
            });
          })
          .catch(error => {
            this.loading = false;
            this.errorMsg = '请求失败';
          })
      }
    }
  }
</script>
<style>
  .card {
    float: left;
    width: 33.333%;
    padding: .75rem;
    margin-bottom: 2rem;
    border: 1px solid #efefef;
    text-align: center;
  }

  .card > img {
    margin-bottom: .75rem;
    border-radius: 100px;
  }

  .card-text {
    font-size: 85%;
  }
</style>
