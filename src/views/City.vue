<template>
  <div class="city">
    <div class="search_wrap">
        <div class="search">
            <i class="fa fa-search"></i>
            <input v-model="city_val" type="text" placeholder="輸入城市名">
        </div>
        <button @click="$router.go(-1)">取消</button>
    </div>
    <div>
        <div class="location">
            <Location :address="city" />
        </div>
        <Alphabet :cityInfo="cityInfo" :keys="keys"/>
    </div>
  </div>
</template>

<script>
import Location from '../components/Location'
import Alphabet from '../components/Alphabet'
export default {
  name: "City",
  data() {
      return {
          city_val: "",
          cityInfo:null,
          keys:[]
      }
  },
  computed: {
    city() {
      return this.$store.getters.location.addressComponent.city || this.$store.getters.location.addressComponent.province;
    }
  },
  components: {
      Location,
      Alphabet
  },
  created() {
      this.getCityInfo();
  },
  methods: {
      getCityInfo() {
          this.$axios("/api/posts/cities")
          .then(res =>{
              //console.log(res.data)
              this.cityInfo = res.data;
              this.keys = Object.keys(res.data)
              this.keys.pop();  //hotcities remove
              this.keys.sort();
          }) 
          .catch(err=>{
              console.log(err)
          })
      }
  }
}
</script>
<style scoped>
.city {
    width: 100%;
    height: 100%;
    overflow: auto;
    box-sizing: border-box;
    padding-top: 45px;
} 
.search_wrap {
    position: fiexed;
    top:0;
    height: 45px;
    width: 100%;
    background: #fff;
    box-sizing: border-box;
    padding: 3px 16px;
    display: flex;
    justify-content: space-between;
}
.search {
    background-color: #eee;
    border-radius: 10px;
    line-height: 40px;
    width: 85%;
    box-sizing: border-box;
    padding: 0 16px;
}
.search input {
    background: #eee;
    outline: none;
    border: none;
    margin-left: 5px;
}
.search_wrap button {
    outline: none;
    color: #009eef;
}
.location {
    background: #fff;
    padding: 8px 16px;
    height: 65px;
    box-sizing: border-box;
}
</style>