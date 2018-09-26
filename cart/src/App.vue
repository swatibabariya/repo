<template>
<div>
<tr v-for="products in product" :key="products">
	<td><img :src='products.src' class="thumbnail" height="100" width="100"></td>
	<td>{{products.price}}</td>
	<td><button @click='add(products)' class='button is-info'>Add to cart</button></td>
	</tr>
  <button @click="prevPage">
    Previous
  </button>
  <button @click="nextPage">
    Next
  </button>
  </div>
</template>
<script>
import { mapGetters, mapActions } from "vuex";
export default {
  data() {
    return {
      pageNumber: 0
    };
  },
  props: {
    product: {
      type: Array,
      required: true
    },
    size: {
      type: Number,
      required: false,
      default: 10
    }
  },
  methods: {
    nextPage() {
      this.pageNumber++;
    },
    prevPage() {
      this.pageNumber--;
    },
    ...mapActions(["addToCart"]),
    add(params) {
      if (this.isLoggedIn) {
        this.addToCart(params);
      } else {
        this.$router.push({
          name: "login",
          query: {
            to: this.$route.fullPath
          }
        });
      }
    }
  },
  computed: {
    ...mapGetters({
      product: "allproduct",
      isLoggedIn: "isLoggedIn"
      // length: "numberofproduct"
    }),
    pageCount() {
      let l = this.product.length,
        s = this.size;
      return Math.floor(l / s);
    },
    paginatedData() {
      const start = this.pageNumber * this.size,
        end = start + this.size;
      return this.product.slice(start, end);
    }
  }
};
</script>