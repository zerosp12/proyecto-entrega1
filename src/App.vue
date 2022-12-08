<template>
  <div id="app">
    <NavbarMenu 
    @showComponent="showComponent($event)"
    :isLogin="isLogin"
    :clientName="clientName"
     />
    <div class="container mt-4 mb-4">
      <ProductsList
        v-show="showProductsList"
        :isLogin="isLogin"
        :productList="productList"
        @addProductToCart="addProductToCart($event)"
        @showComponent="showComponent($event)"
        @verMensaje="verMensaje"
      />
      <AccountForm
        v-show="showAccountForm"
        @loginUser="loginUser"
        @createUser="createUser"
        @verMensaje="verMensaje"
      />
      <CartList
        v-show="showCart"
        :isLogin="isLogin"
        :cartList="cartList"
        @deleteProductToCart="deleteProductToCart($event)"
        @showComponent="showComponent($event)"
      />
      <ModalMessage :Mensaje="MensajeTexto" :Tipo="MensajeTipo" :MostrarMensaje="MostrarMensaje" @cerrarMensaje="cerrarMensaje($event)" />
    
    </div>
  </div>
</template>

<script>
import NavbarMenu from "./components/NavbarMenu.vue";
import ProductsList from "./components/ProductsList.vue";
import AccountForm from "./components/AccountForm.vue";
import CartList from "./components/CartList.vue";
import ModalMessage from "./components/ModalMessage.vue";

export default {
  name: "App",
  components: {
    NavbarMenu,
    ProductsList,
    AccountForm,
    CartList,
    ModalMessage
  },
  data: () => {
    return {  
      //Mensaje
      MostrarMensaje: false,
      MensajeTipo: 0,
      MensajeTexto: '',

      showProductsList: true,
      showAccountForm: false,
      showCart: false,

      //Login User
      userName: "",
      clientName: "",
      isLogin: false,

      //Informacion
      usersList: [
        {
          id: 1,
          username: "jose",
          nombre: "Jose Antonio Destefano",
          password: "1234",
          email: "jose.sp92@gmail.com",
        },
      ],

      cartList: [],

      productList: [
        {
          id: 0,
          nombre: "Robot 1",
          descripcion: "Robot de Prueba 2023",
          precio: 40.5,
          image: "https://robohash.org/0?set=set10&size=300x300",
        },
        {
          id: 1,
          nombre: "Robot 2",
          descripcion: "Robot Versión 2",
          precio: 30.5,
          image: "https://robohash.org/1?set=set10&size=300x300",
        },
        {
          id: 2,
          nombre: "Robot 3",
          descripcion: "Robot Limpiador",
          precio: 20.5,
          image: "https://robohash.org/2?set=set10&size=300x300",
        },
        {
          id: 3,
          nombre: "Robot 4",
          descripcion: "Robot Asesino",
          precio: 20.5,
          image: "https://robohash.org/3?set=set10&size=300x300",
        },
        {
          id: 4,
          nombre: "Robot 5",
          descripcion: "Robot Municipal",
          precio: 20.5,
          image: "https://robohash.org/4?set=set10&size=300x300",
        },
        {
          id: 5,
          nombre: "Robot 6",
          descripcion: "Robot Futbolista",
          precio: 20.5,
          image: "https://robohash.org/5?set=set10&size=300x300",
        },
      ],
    };
  },
  methods: {
    cerrarMensaje() {
        this.MostrarMensaje = false
    },
    verMensaje(info) {
        this.MostrarMensaje = true
        this.MensajeTipo = info[1]
        this.MensajeTexto = info[0]
    },

    loginUser(info) {
      let findUser = this.usersList.find(
        (x) =>
          (x.username.toLowerCase() == info.username.toLowerCase() ||
            x.email.toLowerCase() == info.username.toLowerCase()) &&
          x.password == info.password.toLowerCase()
      );

      if (findUser !== undefined) {
        this.clientName = findUser.nombre;
        this.userName = findUser.username;
        this.isLogin = true;
        this.showProductsList = true;
        this.showAccountForm = false;
        this.showCart = false;
      }
      else
      {
        this.verMensaje(['El usuario / contraseña ingresada es incorrecto', 2])
      }
    },
    createUser(info) {
      if(this.usersList.find((x) => x.email.toLowerCase() == info.email.toLowerCase()) !== undefined)
      {
        this.verMensaje(['El email ya esta en uso', 2])
        return
      }

      if(this.usersList.find((x) => x.username.toLowerCase() == info.username.toLowerCase()) !== undefined)
      {
        this.verMensaje(['El usuario ya esta en uso', 2])
        return
      }

      info.id = this.usersList.length + 1;
      this.usersList.push(info);
      this.verMensaje(['El usuario fue registrado exitosamente', 1])
    },
    addProductToCart(product) {
      let item = this.cartList.find((x) => x.prod_id == product.prod_id);

      if (item == undefined) {
        product.id = this.cartList.length + 1;
        this.cartList.push(product);
      } else {
        let precio_unidad = item.precio / item.cantidad;
        item.cantidad++;
        item.precio = precio_unidad * item.cantidad;
      }
    },
    deleteProductToCart(product) {
      let index = this.cartList.findIndex((x) => x.id == product);

      if (index < 0) return;
      this.cartList.splice(index, 1);
    },
    showComponent(module) {
      switch (module) {
        case 0:
          this.$data.showProductsList = true;
          this.$data.showAccountForm = false;
          this.$data.showCart = false;
          break;

        case 1:
          this.$data.showProductsList = false;
          this.$data.showAccountForm = false;
          this.$data.showCart = true;
          break;

        case 2:
          this.$data.showProductsList = false;
          this.$data.showAccountForm = true;
          this.$data.showCart = false;
          break;

        case 4:
          this.$data.showProductsList = true;
          this.$data.showAccountForm = false;
          this.$data.showCart = false;
          this.$data.isLogin = false;
          window.location.href = "#";
          break;

      }
    },
  },
};
</script>

<style>
html,
body {
  background: #fafafa;
}
#app {
  font-family: "Poppins", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  font-size: 0.95em;
  padding-bottom: 40px
}
.container {
  vertical-align: middle;
}
</style>
