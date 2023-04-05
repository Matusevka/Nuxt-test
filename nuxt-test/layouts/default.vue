
<template>
    <div>
        <Header />
        <Sidebar />
        <nuxt />
    </div>
</template>
  
<script>

import Header from '../components/Header.vue'
import Sidebar from '../components/Sidebar.vue'

export default {
components: {
    Header,
    Sidebar
}
}
</script>

<style>
body{
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
}
</style>