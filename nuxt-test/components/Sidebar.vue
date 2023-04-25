<template>
    <div class="sidebar">
        <div class="sidebar_title">Catalog</div>
        <div class="sidebar_list">
            <div class="sidebar_item" @click="SelectCat('all')">All categories</div>
            <div class="sidebar_item" v-for="item in items" :key="item" @click="SelectCat(item)"><nuxt-link :to="`/`">{{ item }}</nuxt-link></div>
        </div>
    </div>
</template>

<script>

import axios from 'axios';

export default {
    data: function(){
        return{
            origItems: [],
            items: []
        };
    },

    methods:{

        SelectCat(cat){
            this.$emit('usedcat', cat)
            console.log(cat)
        }

        //SelectCat(cat){
        //    this.$parent.$children[2].$children[0].items = this.origItems
        //    this.$parent.$children[2].$children[0].items = this.$parent.$children[2].$children[0].items.filter(e => e.category === cat)
        //}
    },
    mounted(){
        axios.get('https://dummyjson.com/products/categories')
        .then(res => {
            this.items = res.data
            //console.log(this.items)
        })

        axios.get('https://dummyjson.com/products')
            .then(res => {
            this.origItems = res.data.products
        })
    }
} 
</script>


<style scoped>
.sidebar{
    position: fixed;
    left: 0px;
    top: 66px;
    padding-left: 40px;
    height: 100%;
    width: 200px;
    background: #3b3b3b;
}

.sidebar_title{
    font: 1.5em sans-serif;
    color: #fff;
    padding-top: 10px;
}

.sidebar_list{
    margin-top: 15px;
}

.sidebar_item{
    cursor: pointer;
    font-size: 16px;
    line-height: 21px;
    display: flex;
    align-items: flex-end;
    color: #fff;
}
.sidebar_item:hover{
    color: #8e8d8d
}
</style>
