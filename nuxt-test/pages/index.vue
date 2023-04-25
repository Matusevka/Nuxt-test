<template>
  <div class="container">
    <lay @usedcat="UsedCategory" />
    <div class="sort">
      <div class="sorted">
        <div class="sort_title">Sort by:</div>
        <div class="sort_selected" @click="show()">{{ sel }}</div>
      </div>

      <div class="sort_list" :class="{ active: sortActive }">
        <div
          class="sort_list_item"
          @click="
            sel = 'popularity';
            SortFilter(sel);
          "
        >
          popularity
        </div>
        <div
          class="sort_list_item"
          @click="
            sel = 'price';
            SortFilter(sel);
          "
        >
          price
        </div>
      </div>
    </div>
    <div class="cart_list">
      <nuxt-link
        :to="`/product/${item.id}`"
        v-for="item in items.slice(skiped, UsingItems)"
        :key="item.id"
      >
        <Cart :item="item" />
      </nuxt-link>
    </div>
    <div class="pagination">
      <ul
        style="
          width: 100%;
          display: flex;
          flex-direction: row;
          justify-content: center;
          "
      >
        <li v-for="p in totalPages" :key="p" style="list-style-type: none">
          <button
            class="pagination_button"
            @click="
              changePage(p);
              currentPage = p;
            "
            :class="{ active: currentPage === p }"
          >
            {{ p }}
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";

import Header from "../components/Header.vue";
import Sidebar from "../components/Sidebar.vue";
import lay from "../layouts/default.vue";

export default {
  name: "IndexPage",

  components: {
    Header,
    lay,
    Sidebar,
  },

  data: function () {
    return {
      sel: "popularity",
      currentPage: 1,
      total: 0,
      limit: 10,
      sortActive: false,
      items: [],
      skiped: 0,
      origItems: [],
      item: [],
    };
  },
  computed: {
    UsingItems() {
      return this.skiped + this.limit;
    },

    totalPages() {
      return Math.ceil(this.items.length / this.limit);
    },
  },
  methods: {
    UsedCategory(data) {
      if (data === "all") {
        this.items = this.origItems;
      } else {
        this.items = this.origItems;
        this.items = this.items.filter((e) => e.category === data);
      }
      console.log(data)
    },
    changePage(pageNumber) {
      this.$router.push("/" + pageNumber);
      if (pageNumber === 1) {
        this.skiped = 0;
        this.currentPage = pageNumber;
      } else {
        this.skiped = pageNumber * this.limit - this.limit;
      }
    },

    show() {
      this.sortActive = !this.sortActive;
    },

    SortFilter(e) {
      if (e === "price") {
        this.items = this.items.sort((a, b) => b.price - a.price);
        this.$refs.sel = `price`;
      } else {
        this.items = this.items.sort((a, b) => b.rating - a.rating);
        this.$refs.sel = `popularity`;
      }
    },
  },

  mounted() {
    //this.changePage(this.currentPage)

    axios.get("https://dummyjson.com/products").then((res) => {
      this.items = res.data.products;
      this.origItems = this.items;
      this.total = res.data.total;
    });
  },
};
</script>

<style scoped>
.sorted {
  display: flex;
  flex-direction: row;
}
* {
  text-decoration: none;
}
.pagination {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
}
.pagination_button {
  background: #fff;
  color: #1c1d21;
  border: 1px solid silver;
  cursor: pointer;
}
.pagination_button.active {
  background: #a5a5a5;
  cursor: default;
}
.container {
  margin-left: 240px;
  margin-top: 120px;
}
.cart_list {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  margin-right: 105px;
}
.sort_list {
  display: none;
  top: 0;
  left: 0;
  width: 100%;
  background: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
  border-radius: 8px;
}
.sort_list.active {
  display: contents;
}
.sort {
  position: fixed;
  bottom: 100px;
  right: 80px;
  display: flex;
  flex-direction: column;
  z-index: 101;
}
.sort_selected {
  padding-left: 3px;
  color: #59606d;
  cursor: pointer;
  position: relative;
}
.sort_list_item {
  background: #f8f8f8;
  color: #959dad;
  cursor: pointer;
  padding: 4px 4px 0 12px;
}
.sort_list_item:hover {
  color: #1f1f1f;
}
</style>
