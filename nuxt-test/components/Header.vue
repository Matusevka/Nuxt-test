<template>
    <div class="header">
        
        <nuxt-link to="/">
            <div class="header_logo">NuxtTest</div>
        </nuxt-link>

        <div class="header_search">
            <input class="search_input" v-model="message" v-on:keyup="search(message)"
                type="text" maxlength= "12" placeholder="Search">
            <div class="founded">
                <a v-for="item in items.slice(0, 5)" :key="item.id" href="" @click.prevent="goTo(item.id)">
                    <div class="item">{{item.title}}</div>
                </a>
            </div>
        </div>
    </div>
</template>

<script>

import axios from 'axios';

export default {
    data: function(){
        return{
            origItems: [],
            items: [],
            message: ''
        };
    },

    methods:{
        goTo(item){
          console.log(item)
          this.$router.push('/product/' + item)
          this.message = ''
          this.items = []
        },

        search(a){
            if(a !== '')
            axios.get('https://dummyjson.com/products/search?q='+a)
            .then(res => {
                this.items = res.data.products
                console.log(this.items)
            })
            else{
                this.items = []
            }
        }
    },
    mounted(){
        
    }
} 
</script>

<style>
.founded{
    position: absolute;
    border-radius: 0 0 28px 28px;
    cursor: pointer;
    z-index: 110;
}
.item{
    color: black;
    width: 180px;
    background: #fff;
    box-shadow: 0 0 20px rgba(0,0,0,.2);
    padding: 8px 4px 13px 8px;
    z-index: 110;
}
.item:hover{
    background: #c4c4c4;
}
.header{
    position: fixed;
    width: 100%;
    height: 66px;
    left: 0px;
    top: 0px;
    background: #3b3b3b;
    border-radius: 0px 0px 8px 0px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    z-index: 99;
}
.header_logo{
    color: #fff;
    margin-left: 40px;
    font: 2em sans-serif;
    font-weight: bold;
}
.header_search{
    color: #fff;
    margin-right: 40px;
}
.search_input{
    border-radius: 10px;
    height: 20px;
    width: 180px;
}
.search_input:active, :hover, :focus {
    outline: 0;
    outline-offset: 0;
}
</style>