<template>
  <div v-if="isLoading" class="loader"></div>
  <div v-else class="card">
    <div class="kiri">
      <img :src="currentProduct.image" :alt="currentProduct.title" />
    </div>
    <div class="kanan">
      <div class="tulisan">
        <h1 :class="getCategoryClass(currentProduct.category)">
          {{ currentProduct.title }}
        </h1>
        <p>{{ currentProduct.category }}</p>
        <h3>{{ currentProduct.description }}</h3>
        <h1 :class="getCategoryClass(currentProduct.category)">
          $ {{ currentProduct.price }}
        </h1>
      </div>
      <div class="tombol">
        <button :class="getCategoryClass(currentProduct.category)">
          Buy Now
        </button>
        <button
          @click="showNextProduct"
          :class="getCategoryClass(currentProduct.category)"
        >
          Next Product
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HalamanProduk",
  data() {
    return {
      products: [],
      currentProductIndex: 0,
      isLoading: false,
      currentCategory: "",
    };
  },
  created() {
    this.fetchData();
  },

  methods: {
    fetchData() {
      axios
        .get("https://fakestoreapi.com/products")
        .then((response) => {
          this.products = response.data;
          this.currentCategory =
            this.products[this.currentProductIndex].category;
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    },

    // fungsi looping untuk produk
    showNextProduct() {
      this.isLoading = true;
      setTimeout(() => {
        if (this.currentProductIndex < this.products.length - 1) {
          this.currentProductIndex++;
        } else {
          this.currentProductIndex = 0;
        }
        this.currentCategory = this.products[this.currentProductIndex].category;
        this.isLoading = false;
      }, 300);
    },

    // fungsi class binding untuk kustomisasi style
    getCategoryClass(category) {
      return {
        "category-style-1": category === "men's clothing",
        "category-style-2": category === "electronics",
        "category-style-3": category === "women's clothing",
        "category-style-4": category === "jewelery",
      };
    },

    setBodyBackgroundColor() {
      document.body.style.backgroundColor = this.getCategoryBackgroundColor(
        this.currentCategory
      );
    },

    getCategoryBackgroundColor(category) {
      if (category === "men's clothing") {
        return "#d6e6ff";
      } else if (category === "electronics") {
        return "#d6e6ff";
      } else if (category === "women's clothing") {
        return "#fde2ff";
      } else if (category === "jewelery") {
        return "#fde2ff";
      }

      return "#dcdcdc";
    },
  },

  computed: {
    currentProduct() {
      return this.products[this.currentProductIndex] || {};
    },
  },

  watch: {
    currentCategory() {
      // Fungsi yang akan dijalankan saat currentCategory berubah
      this.setBodyBackgroundColor();
    },
  },
};
</script>

<style src="../assets/style/page.css"></style>
