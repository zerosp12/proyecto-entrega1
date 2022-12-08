<template>
  <div>
    <ProductInfo v-if="showProductInfo" :productInfo="productInfo" @backToList="backToList" />
    <div v-else>
      <h1 class="pt-4">Lista de Productos</h1>
      <hr class="mt-4 mb-4" />
      <div
        v-if="showLoginMessage"
        class="alert alert-danger alert-dismissible fade show"
        role="alert"
      >
        <i class="fas fa-exclamation-circle"></i> Para agregar productos al
        carrito debes iniciar sesión desde la sección <strong>Cuenta</strong>
      </div>
      <div class="row row-cols-1 row-cols-md-n g-4">
        <div class="col-lg-3" v-for="(product, index) in productList" :key="index">
          <div class="card text-center">
            <img
              :src="product.image"
              class="card-img-top"
              alt=""
              style="width: 80%; margin: 0px auto"
            />
            <div class="card-body">
              <h6 class="card-title fw-bold">{{ product.nombre }}</h6>
              <br>
              <h3 class="pt-3 pb-2">$ {{ product.precio }}</h3>
              <hr />
              <button
                class="btn btn-warning btn-sm"
                @click="addProductToCart(product.id, $event)"
              >
                <i class="fas fa-shopping-cart mr-2"></i> Agregar al Carrito
              </button>
              <br>
              <br>
              <button type="button" class="btn btn-primary btn-sm" @click="showInfo(product)">Más Información</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ProductInfo from '@/components/ProductInfo.vue'

export default {
  name: "ProductList",
  components: {
    ProductInfo
  },
  data: () => {
    return {
      showLoginMessage: false,
      showProductInfo: false,
      productInfo: []
    };
  },
  
  props: {
    isLogin: Boolean,
    productList: Array,
  },
  methods: {
    backToList() {
      this.showProductInfo = false
    },
    showInfo(product) {
      this.productInfo = product
      this.showProductInfo = true
    },
    showComponent(component) {
      this.$emit("showComponent", component);
    },
    addProductToCart(index, event) {
      if (this.isLogin == false) {
        this.$emit('verMensaje', ['Debes iniciar sesión para agregar productos al carrito', 2])
        return;
      }

      let product = this.productList.find((x) => x.id === index);
      if (product === undefined) return;

      var target = event.currentTarget;
      target.innerHTML = '<i class="fas fa-check mr-2"></i> <b>Agregado!</b>';

      this.$emit("addProductToCart", {
        id: 0,
        prod_id: product.id,
        nombre: product.nombre,
        precio: product.precio,
        cantidad: 1,
      });

      setTimeout(() => {
        target.innerHTML =
          '<i class="fas fa-shopping-cart mr-2"></i> Agregar al Carrito';
      }, 300);
    },
  },
};
</script>

<style scoped>
.card {
  border: 0px !important;
  padding: 20px 0px 20px 0px;
  box-shadow: 0px 0px 6px 2px #eaeaea;
  transition: box-shadow 0.3s ease-in-out;
}

.card:hover {
  cursor: pointer;
  box-shadow: 0px 0px 6px 2px #bbb;
}
</style>
