<template>
    <div class="item" :cla="id">
        <div class="title">{{title}}</div>
        <div class="content" :style='"background-color:"+content'>{{value}}</div>
    </div>
</template>
<script>
export default {
    name:"Item",
    props:{
        title: Number,
        content: String,
        id: Number,
        value: String
    }
}
</script>
<style>
    .item {
        height:150px;
        text-align: center;
        border: 2px solid;
        margin: 5px 0px;
    }
    .title {
        font-size: 17pt;
    }
    .content {
        height: 76.5%;
        color: aliceblue;
    }
    
</style>