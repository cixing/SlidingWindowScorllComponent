<template>
    <div class="all">
       <Item v-for="(item,i) in items" :title="item.title" :value="item.value" :content="item.content" :key="i" :id="i" ></Item>
    </div>
</template>
<script>
import Item from './item.vue';
import List from './constant.js';
let start = 0;
let end = 14;
let shoudUpdate = false;    //标志位，避免反复调用update周期函数
let direction = 'down';     //获取数据的方向
const MaxIndex = List.length;
const loadNum = 5;

const getData = (start,end,direction)=> {
          if(direction === 'down') {
             if(end <= MaxIndex-1) {
             return List.slice(end-loadNum+1,end+1);
             }
             else {
                 return List.slice(end-loadNum+1);
             }
          }
          else {
              if(start >=0) {
                  return List.slice(start, start+loadNum);
              }
              else {
                  return List.slice(0,start+loadNum);
              }
          }
        }
 const options = {
          root: null,
          rootMargin: '0px',
          threshold: [0,0.1]
        };
export default {
    name: 'Scroll',
    components: {
        Item
    },
    data: function() {
        return {
            items:[
            {title:1,value: 'A',content:'#ff0000'},
            {title:2,value: 'B',content:'#00ff00'},
            {title:3,value: 'C',content:'#0000ff'},
            {title:4,value: 'D',content:'#FFFF00'},
            {title:5,value: 'E',content:'#00f0ff'},
            {title:6,value: 'F',content:'#ff0000'},
            {title:7,value: 'G',content:'#00ff00'},
            {title:8,value: 'H',content:'#0000ff'},
            {title:9,value: 'I',content:'#FFFF00'},
            {title:10,value: 'J',content:'#00f0ff'},
            {title:11,value: 'K',content:'#ff0000'},
            {title:12,value: 'L',content:'#00ff00'},
            {title:13,value: 'M',content:'#0000ff'},
            {title:14,value: 'N',content:'#FFFF00'},
            {title:15,value: 'O',content:'#00f0ff'},
            ],
            isInter: false
        }
    },
    mounted(){

        let down = document.querySelectorAll('[cla]')[end];
        let all = document.querySelector('.all');
        let items = this.items;
        let isInter = this.isInter;
        const Observer = new IntersectionObserver(function(entries){
            if (entries[0].intersectionRatio < 0.1 ) return;
            for(let i = 0; i < loadNum; i++) {
              items.shift();
            }
            start = start+loadNum;
            end = end+loadNum;
            let distance = start*150;
            all.style.transform = 'translateY('+distance+'px)';
            let data = getData(start,end,'down');
            items.push.apply(items,data);   
            console.log('Loaded new items');
            Observer.unobserve(down);
            shoudUpdate = true;
        }, options);
        Observer.observe(down);
    },
    updated(){
        if(!shoudUpdate) {
            return;
        }
       shoudUpdate = false; 
       let items = this.items;
       let down = document.querySelectorAll('[cla]')[14];
       let up = document.querySelectorAll('[cla]')[0];
       let all = document.querySelector('.all');
       down.id = 'bottom';
       up.id = 'top';
   
       const Observer = new IntersectionObserver(function(entries){
           entries.forEach((entry, index) => {
               if (entry.isIntersecting && entry.target.id === "bottom") {
                   if(end >= MaxIndex-1) {
                       return;
                   }
                   Observer.unobserve(up);
                   
                   start = start+loadNum;
                   end = end+loadNum;
                   let distance = start*150;
                   all.style.transform = 'translateY('+distance+'px)';
                   let data = getData(start,end,'down');
                   items.push.apply(items,data);

                   for(let i = 0; i < data.length;i++) {
                       items.shift();
                   }
                   
                   console.log('Loaded down items');
                   Observer.unobserve(down);
                   shoudUpdate = true;
                   }
                if(entry.isIntersecting && entry.target.id === "top") {
                    if(start <=0) {
                       return;
                   }
                   Observer.unobserve(down);
                   
                   start = start-loadNum;
                   end = end-loadNum;
                   let distance = start*150;
                   if(distance < 0) {
                       distance = 0;
                   }
                   all.style.transform = 'translateY('+distance+'px)';
                   let data = getData(start,end,'up');
                   items.unshift.apply(items,data);
                   for(let i = 0; i < data.length;i++) {
                       items.pop();
                   }
                   
                   console.log('Loaded up items');
                   Observer.unobserve(up);
                   shoudUpdate = true;
                   }               
               })
           },options);
        Observer.observe(down);
        Observer.observe(up);
    }
}
</script>
<style>
    .all {
        height: 750px;
    }
</style>