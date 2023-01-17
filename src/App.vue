<template>
  <div id="app">
    <h1 class="card-header ml-3 mr-3 text-center">
      V6 To V7
    </h1>
    <div class="container">
      <div class="row">
        <div class="col">
          <h5 class="card-header">V6 title</h5>
          <div class="card-body text-secondary">
            <textarea rows="40" cols="45" class="txt" placeholder="Enter the value" v-model="v6myjson">  </textarea>
          </div>
        </div>
        <div class="col">
          <h5 class="card-header">V7 title</h5>
          <div class="card-body text-secondary">
            <textarea rows="40" cols="45" placeholder="Enter the value" v-model="v7newmyjson">  </textarea>
          </div>
        </div>
      </div>
      <div class="word">
        <button type="submit" class="btn btn-success mt-0 mb-3" @click="convert">Convert</button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {

  },
  data(){
    return{
      v6myjson:'',
      v7newmyjson:'',
      myjson:Object,
      newmyjson:Object,
    }
  },
  methods:{
    convert(){
      var search1 ='window.sq = window.sq || {}; window.sq.config = Object.assign('
      var search2 =', window.sq.config);'
      var n = this.v6myjson.replace(/^\s+|\s+$/g, "");
      var a = n.replace(search1,"").replace(search2,"")

      this.myjson = JSON.parse(a)
      this.newmyjson = this.myjson

      // searchFields value string to Array
      this.newmyjson.searchFields = this.myjson.searchFields.split(",")


      // deleting facets
      delete this.newmyjson.facets



      //remove pricefilters field
      // (if filter type is 3 ,
      // then the value of priceFilters will be embedded to the ranges key of that object)

      var arr = JSON.parse(this.newmyjson.filters)
      // console.log(arr)
      var index = 0
      for(index in arr){
        if(arr[index].filterType == 3){
          arr[index].ranges = JSON.parse(this.myjson.priceFilters)
        }
      }
      this.newmyjson.filters = arr

      delete this.newmyjson.priceFilters

      this.v7newmyjson = search1 + JSON.stringify(this.newmyjson) +search2
    },
  }
}
</script>

<style>

.word{
  margin:1vw;
}
.btn{
  width: 100%;
}
.col{
  border-right: 1px solid lightgrey;
  border-left: 1px solid lightgrey;
}
.row {
  border: 1px solid lightgrey;
  margin: 1vw;
  height: 72.5vh;
  overflow: auto;
}
</style>
