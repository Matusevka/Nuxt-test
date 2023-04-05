<template>
  <div class="container">
    <div class="sort">

      <div class="sorted">
        <div class="sort_title">Sort by:</div>
        <div class="sort_selected" @click="show();" ref="selectedText">popularity</div>
      </div>

      <div class="sort_list" :class="{ active: sortActive }">
        <div class="sort_list_item" @click="sel = 'popularity'; SortFilter(sel)">popularity</div>
        <div class="sort_list_item" @click="sel = 'price'; SortFilter(sel)">price</div>
      </div>
    </div>
    <div class="cart_list">
      <a href="" v-for="item in items.slice(skiped, skiped+limit)" :key="item.id" @click.prevent="goTo(item.id)">
        <Cart :item="item" />
      </a>
    </div>
    <div class="pagination">
      <ul style="width: 100%; display: flex; flex-direction: row; justify-content: center;">
        <li v-for="p in totalPages" :key="p" style="list-style-type: none;">
          <button 
            class="pagination_button" 
            @click="changePage(p)"
            :class="{active: currentPage === p}">
            {{ p }}
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  name: 'IndexPage',

  data: function(){
        return{
            sel: 'popularity',
            currentPage: 1,
            total: 0,
            limit: 1,
            sortActive: false,
            items: [],
            skiped: 0,
            item: []
        };
    },
    computed: {
        totalPages() {
          return Math.ceil(this.items.length / this.limit)
        },
    },
    methods:{
        changePage(pageNumber){
          if (pageNumber === 1){
            this.skiped = 0
          }
          else{
            this.skiped = pageNumber * this.limit - this.limit
          }
        },

        show(){
          this.sortActive = !this.sortActive
        },
        
        goTo(product){
          this.$router.push('/product/' + product)
        },

        SortFilter(e){
          if(e === 'price'){
            this.items = this.items.sort((a,b) => b.price - a.price)
            this.$refs.selectedText.textContent = `price`
          }
          else{
            this.items = this.items.sort((a,b) => b.rating - a.rating)
            this.$refs.selectedText.textContent = `popularity`
          }
        }
    },

    mounted(){

        this.SortFilter(this.sel)
      
        axios.get('https://dummyjson.com/products')
        .then(res => {
            this.items = res.data.products
        })

        axios.get('https://dummyjson.com/products?limit=10&skip='+ this.skiped +'&select=title,price')
        .then(res => {
          this.total = res.data.total
          this.limit = res.data.limit
        })
    }
}
</script>

<style>
.sorted{
  display: flex;
  flex-direction: row;
}
*{
  text-decoration: none;
}
.pagination{
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
}
.pagination_button{
  background: #fff;
  color: #1c1d21;
  border: 1px solid silver;
}
.container{
  margin-left: 240px;
  margin-top: 120px;
}
.cart_list{
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  margin-right: 105px;
}
.sort_list{
  display: none;
  top: 0;
  left: 0;
  width: 100%;
  background: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
  border-radius: 8px;
}
.sort_list.active{
  display: contents;
}
.sort{
  position: fixed;
  bottom: 100px;
  right: 80px;
  display: flex;
  flex-direction: column;
  z-index: 101;
}
.sort_selected{
  padding-left: 3px;
  color: #59606d;
  cursor: pointer;
  position: relative;
}
.sort_list_item{
  background: #f8f8f8;
  color: #959dad;
  cursor: pointer;
  padding: 4px 4px 0 12px;
}
.sort_list_item:hover{
  color: #1f1f1f;
}
</style>

