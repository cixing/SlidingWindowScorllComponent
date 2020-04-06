
<template>
    <Scroll></Scroll>
</template>
<script>

import Scroll from './scroll.vue'
export default {
  name: 'App',
  components: {
      Scroll
  },
  methods: {
      dataChange(name, value) {
          data[name] = value;
      },
      submit() {
          console.log(data);
      }
   },
   data: function(){
       return {
    list:[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]
     }
   }
}

</script>
<style>
    .submit
        {
        text-align: center;
        margin-top: 30px;
    }
    .col{
        padding-left: 15px;
    }
    .bg {
        background-color: #F5FFFA
    }
</style>