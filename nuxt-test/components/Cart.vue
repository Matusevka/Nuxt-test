<template>
    <div class="cart" :data-cart="this.$props.category">
        <div class="discount">{{ item.discountPercentage }}%</div>
        <div class="cart_raiting">{{ item.rating }}</div>
        <img
            class="cart_img"
            loading="lazy" 
            :src="item.images[0]">
        <div class="cart_title">{{ item.title }}</div>
        <div class="cart_price">{{ item.price }}$</div>
    </div>
</template>

<script>

export default {

    props: ['item'],

}
</script>

<style scoped>
.discount{
    position: absolute;
    color: #fff;
    background-color: #f70000;
    width: auto;
    top: 0px;
    right: 0px;
    border-radius: 4px;
    margin: 10px 0px 0 10px;
    padding: 3px;
    z-index: 99;
}
.cart_raiting{
    color: rgb(235, 235, 1);
    top: 0px;
    left: 0px;
    position: absolute;
}
.cart{
    width: 225px;
    height: 240px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
    padding: 16px;
    border-radius: 4px;
    cursor: pointer;
    position: relative;
    display: flex;
    justify-content: flex-end;
    flex-direction: column;
    margin: 16px;
    z-index: 98;
    background: #fff;
}
.cart_img{
    width: 150px;
    position: absolute;
    top: 0px;
    left: 0px;
    right: 0px;
    bottom: 0px;
    margin: auto;
}
.cart_price{
    font-weight: bold;
    font-size: 14px;
    font-family: sans-serif;
    color: #1f1f1f;
}

</style>