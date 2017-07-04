<template>
  <div id="app">
    <div id="wrapper" @scroll="onScroll">
      <ul class="list">
        <section class="grid">
          <div class="grid_item card" v-for="photo in photos">
            <div class="card_body">
              <img :src="photo.urls.small" alt="">
            </div> <!-- card_body -->
            <div class="card_footer media">
              <img :src="photo.user.profile_image.small" class="media_obj" alt="">
              <div class="media_body">
                <a href="javascript:void(0)" target="_blank">{{ photo.user.name }}</a>
              </div> <!-- media_body -->
            </div> <!-- card_footer -->
          </div> <!-- grid_item -->
        </section> <!-- grid -->
      </ul> <!-- ul.list -->
    </div>
  </div>
</template>

<script>
const appID = '31a48cc863e74789b0eec0bce2bbbcd2bbccdaf3cf28c97eac4d7b2e16baf2f3';
export default {
  name: 'app',
  data () {
    return {
      photos: [],
      totalPhotos: 0,
      perPage: 10,
      currentPage: 1,
      loading: false  
    }
  },
  methods:{
    onScroll(event){
      const wrapper = event.target; // wrapper
      const list = wrapper.firstElementChild; // list
      const scrollTop = wrapper.scrollTop;
      const wrapperHeight = wrapper.offsetHeight;
      let listHeight = list.offsetHeight;
      let diffHeight = listHeight - wrapperHeight;
      if(diffHeight <= scrollTop && !this.loading){
        this.fetchPhotos(this.currentPage);
      }

    },
    fetchPhotos(page){
      this.loading = true;
      var options = { 
        params: {
          client_id : appID,
          page: page,
          per_page: this.perPage
        }
      };
      this.$http.get('https://api.unsplash.com/photos', options)
      .then(function(response){
        this.photos = this.photos.concat(response.data);
        this.totalPhotos = parseInt(response.headers.get('x-total'));
        this.currentPage++;
        this.loading = false;
      }, function(error){
        console.log(error)
      })
    }
  },
  created(){
    this.fetchPhotos(this.currentPage);
  }
}
</script>

<style lang="scss">
  *{
    box-sizing: border-box;
  }
  body{
    margin: 0;
    padding: 0;
    background: linear-gradient(to right, #79d695, #5defe2);
  }
  #app{
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }
  #wrapper{
    height: 700px;
    width: 500px;
    background: #fff;
    overflow-y: scroll;
  }
  .list{
    padding: 10px 0 0 0;
    margin: 0;
    list-style: none;
  }
  .grid{
    width: 100%;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    margin: 0 auto;
  }
  .grid_item{
    width: 100%;
    flex-grow: 1;
    flex-shrink: 1;
    margin: 0 20px 40px;
  }
  .card{
    background-color: #fff;
    overflow: hidden;
    box-shadow: 0 1px 2px rgba(0,0,0,.2);
    border-radius: 2px;
    line-height: 0;
  }
  .card:hover{
    box-shadow: 0 3px 6px rgba(0,0,0,.2);
  }
  .card_body{
    width: 100%;
    height: 215px;
    overflow: hidden;
    background-color: #eee;
  }
  .card_body img{
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .card_footer{
    width: 100%;
    padding: 10px 15px;
  }
  .media_obj{
    width: 32px;
    height: 32px;
    border-radius: 50%;
    background-color: #d8d8d8;
    margin-right: 15px;
    float: left;
  }
  .media_body{
    width: 100%;
    height: 32px;
    line-height: 32px;
  } 
  .media_body a{
    font-size: 15px;
    color: #999;
  }
  .media_body a:hover{
    text-decoration: none;  
  }
</style>

