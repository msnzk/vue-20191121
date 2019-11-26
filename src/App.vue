<template>
  <div id="app">
    <div class="toolbar">
      <p class="title">Image Search</p>
      <div class="search">
          <input type="text" v-model="searchword" placeholder="キャンペーン名、掲載カテゴリー、ポイント数">
      </div>
    </div>


    <div class="content">
      <div class="banners" v-for="(banner, key) in filteredBanners" v-bind:key="key">
        <div class="banner-info">
          <p class="project">{{banner.project}}</p>
          <p class="label" v-if="banner.label">{{banner.label}}</p>
          <p class="category" v-if="banner.category">{{banner.category}}</p>
        </div>
        <ul class="banner-list">
          <li v-for="(item, key2) in banner.items" v-bind:key="key2"><img v-bind:src="item.url" alt=""><span>{{item.size}}</span></li>
        </ul>
      </div><!-- /.banners -->

    </div><!-- /.content -->

  </div>
</template>

<script>
import axios from 'axios'

var url = "./banners.json";
export default {
  el: '#app',
  data () {
    return {
      banners: null,
      searchword: ''
    }
  },
  mounted () {
    axios
      .get(url) //ここにURLを入れる
      .then(response => (this.banners = response.data))
  },
  computed: {
    filteredBanners: function() {

      var banners = [];

      for(var i in this.banners) {

            var banner = this.banners[i];
            var str = (banner.project + banner.category + banner.label).toLowerCase();
            var word = this.searchword.toLowerCase();

            if(str.indexOf(word) !== -1) {
                banners.push(banner);
            }
        }
        return banners;
    }
  }
}
</script>

<style lang="scss">
body {
  color: #fff;
  background-color: #212121;
  .sample {
    bottom: 0;
  }
}
.toolbar {
  padding: 40px;
  background: #000;
  text-align: center;
}
.title {
  color: #fff;
  font-weight: normal;
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 40px;
}
.search input {
  background-color: #666;
  color: #fff;
  padding: 0 1em;
  border-radius: 4px;
  margin-right: 12px;
  height: 48px;
  width: 600px;

}
.search input::placeholder {
  font-size: 14px;
  color: rgba(255,255,255,0.7);
}

.msg-result {
  margin-top: 40px;
  text-align: center;
}
.msg-result em {
  font-weight: bold;
  font-size: 1.5em;
}

.banners {
  display: flex;
  width: 90%;
  margin: 40px auto;
  border: 1px solid #666;
  box-shadow: 0 0 10px #000;
  border-radius: 8px;
  padding: 24px;
  position: relative;
}
.banner-info {
  border-right: 1px solid #888;
  width: 200px;
  padding-right: 24px;
}
.banner-info .project {
  font-size: 20px;
  font-weight: bold;
  margin-top: 40px;
  line-height: 1.5;
}
.banner-info .label {
  margin-top: 24px;
  font-size: 14px;
  color: #ffff00;
}
.banner-info .category {
  background-color: #009688;
  position: absolute;
  top: 0;
  left: 0;
  font-size: 14px;
  font-weight: bold;
  padding:0.5em 1em;
  border-radius: 6px 0 6px 0;
}
.banner-list img {
  max-width: 100%;
  max-height: 100%;
}
.banner-list {
	flex: 1;
  display: flex;
  flex-wrap: wrap;
  padding:0 0 0 12px;
}
.banner-list li{
  width: 180px;
  height: 180px;
  display: flex;
  align-items: center;
  background-color: #444;
  margin: 0 12px 24px;
  list-style-type: none;
  position: relative;
}
.banner-list li span {
  display: block;
  background-color: rgba(0,0,0,0.2);
  font-size: 10px;
  padding: 0.5em 1em;
  text-align: right;
  position: absolute;
  bottom: 0;
  width: 100%;
}

</style>
