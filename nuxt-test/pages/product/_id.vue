<template>
    <div class="container">  
        <div class="prod">
            <div class="left_column">
                <img class="photo"
                    loading="lazy" 
                    :src="product.thumbnail">
            </div>

            <div class="right_column">
                <div class="product_description">
                    <span>{{product.category}}</span>
                    <h1>{{ product.title }}</h1>

                    {{ product.description }}
                    <div class="manufacture">
                        <h2>Manufacturer:</h2>
                        <span>{{ product.brand }}</span>
                    </div>
                    
                    <div class="product-price"><span>{{product.price}}$</span> <button class="cart-btn" href="">Add to cart</button></div>
                    Stock:
                    {{ product.stock }}
                </div>
            </div>
        </div>


    </div>
</template>

<script>

import axios from "axios"

export default({
    name: 'ProductPage',

    data: function(){
        return{
            product: [],
        };
    },
    mounted(){
        axios.get('https://dummyjson.com/products/'+ this.$route.params.id)
        .then(res => {
          this.product = res.data
          console.log(this.product)
        })
    }
})
</script>

<style scoped>
.product_description {
  border-bottom: 1px solid silver;
  margin-bottom: 20px;
}
span{
    padding-left: 10px;
    font-size: 24px;
}
.manufacture{
    display: flex;
    flex-direction: row;
    align-items: center;
}
h2 {
  font-size: 24px;
  text-transform: uppercase;
  text-decoration: none;
}
h1 {
  font-weight: 300;
  font-size: 52px;
  color: #43484D;
  letter-spacing: -2px;
}
p {
  font-size: 32px;
  font-weight: 300;
  color: #86939E;
  line-height: 24px;
}
.right_column {
    width: 35%;
    margin-top: 60px;
    padding-left: 20px;
}
.left_column {
  width: 65%;
  position: relative;
}
.photo {
  width: 100%;
  left: 0;
  top: 0;
}
.prod{
    display: flex;
}
*{
    text-decoration: none;
}
.container{
  margin-left: 240px;
  margin-top: 120px;
}
.product-price {
  display: flex;
  align-items: center;
}
  
.product-price span {
  font-size: 24px;
  font-weight: 300;
  color: #43474D;
  margin-right: 20px;
}
  
.cart-btn {
  display: inline-block;
  background-color: #7DC855;
  border-radius: 6px;
  font-size: 16px;
  color: #FFFFFF;
  text-decoration: none;
  padding: 12px 30px;
  transition: all .5s;
  border: 1px solid silver
}
.cart-btn:hover {
  background-color: #64af3d;
}
</style>

//нужно понять почему нельзя перейти на категорию из этого