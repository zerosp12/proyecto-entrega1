<template>
  <div class="form-content">
    <ul class="nav nav-pills nav-justified mb-3" id="ex1" role="tablist">
      <li class="nav-item" role="presentation">
        <a
          :class="LoginTabCSS"
          id="tab-login"
          data-mdb-toggle="pill"
          @click="showLogin"
          href="#"
          role="tab"
          aria-controls="pills-login"
          aria-selected="true"
          >Ingresar</a
        >
      </li>
      <li class="nav-item" role="presentation">
        <a
          :class="RegisterTabCSS"
          id="tab-register"
          data-mdb-toggle="pill"
          @click="showRegister"
          href="#"
          role="tab"
          aria-controls="pills-register"
          aria-selected="false"
          >Crear Cuenta</a
        >
      </li>
    </ul>
    <!--Login-->
    <div class="tab-content">
      <div
        v-if="ShowLogin"
        class="tab-pane show active"
        id="pills-login"
        role="tabpanel"
        aria-labelledby="tab-login"
      >
        <h1 class="pt-4">Ingresar</h1>
        <hr class="mt-4 mb-4" />
        <form>
          <div class="form-outline mb-4">
            <label class="form-label" for="loginName">Email o Usuario</label>
            <input
              v-model="loginUsername"
              type="text"
              id="loginName"
              class="form-control"
            />
          </div>
          <div class="form-outline mb-4">
            <label class="form-label" for="loginPassword">Contraseña</label>
            <input
              v-model="loginPassword"
              type="password"
              id="loginPassword"
              class="form-control"
            />
          </div>

          <button
            @click="loginRequest"
            class="btn btn-primary btn-block mb-4"
          >
            Ingresar
          </button>

          <div class="text-center">
            <p>
              No eres miembro? <a @click="showRegister" href="#">Registrate</a>
            </p>
          </div>
        </form>
      </div>
      <!--Registro-->
      <div
        v-else
        class="tab-pane show active"
        id="pills-register"
        role="tabpanel"
        aria-labelledby="tab-register"
      >
        <h1 class="pt-4">Registrate</h1>
        <hr class="mt-4 mb-4" />
        <form>
          <label class="form-label" for="registerName">Nombre / Apellido</label>
          <div class="form-outline mb-4">
            <input
              type="text"
              id="registerName"
              class="form-control"
              v-model="registerName"
            />
          </div>
          <div class="form-outline mb-4">
            <label class="form-label" for="registerUsername">Usuario</label>
            <input
              type="text"
              id="registerUsername"
              class="form-control"
              v-model="registerUsername"
            />
          </div>
          <div class="form-outline mb-4">
            <label class="form-label" for="registerEmail">Email</label>
            <input
              type="email"
              id="registerEmail"
              class="form-control"
              v-model="registerEmail"
            />
          </div>
          <div class="form-outline mb-4">
            <label class="form-label" for="registerPassword">Contraseña</label>
            <input
              type="password"
              id="registerPassword"
              class="form-control"
              v-model="registerPassword"
            />
          </div>
          <div class="form-outline mb-4">
            <label class="form-label" for="registerRepeatPassword"
              >Repite Contraseña</label
            >
            <input
              type="password"
              id="registerRepeatPassword"
              class="form-control"
              v-model="registerRepeatPassword"
            />
          </div>
          <button 
            class="btn btn-success btn-block mb-3"
            @click="createRequest()"
          >
            Registrar Cuenta
          </button>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
export default {

  name: "LoginForm",

  data: () => {
    return {
      //Login
      loginUsername: "",
      loginPassword: "",

      //Crear cuenta
      registerName: "",
      registerUsername: "",
      registerEmail: "",
      registerPassword: "",
      registerRepeatPassword: "",

      ShowLogin: true,

      LoginTabCSS: "nav-link active",
      RegisterTabCSS: "nav-link",
    };
  },
  methods: {
    
    loginRequest(event) {

      event.preventDefault();

      if(this.loginUsername === '')
      { 
        this.$emit('verMensaje', ['Debes ingresar el usuario / email', 2])
        return
      }

      if(this.loginPassword === '')
      { 
        this.$emit('verMensaje', ['Debes ingresar la contraseña', 2])
        return
      }

      this.$emit("loginUser", {
        username: this.loginUsername,
        password: this.loginPassword,
      });
    },
    createRequest(event) {

      event.preventDefault();

      if(this.registerName === '')
      { 
        this.$emit('verMensaje', ['Debes ingresar un Nombre', 2])
        return
      }

      if(this.registerUsername === '')
      { 
        this.$emit('verMensaje', ['Debes ingresar un Usuario', 2])
        return
      }

      if(this.registerEmail === '')
      { 
        this.$emit('verMensaje', ['Debes ingresar un Email', 2])
        return
      }
      
      if(this.registerPassword === '')
      { 
        this.$emit('verMensaje', ['Debes ingresar una Contraseña', 2])
        return
      }

      if(this.registerPassword !== this.registerRepeatPassword)
      { 
        this.$emit('verMensaje', ['Las contraseñas no Coinciden', 2])
        return
      }

      this.$emit("createUser", {
        id: 0,
        nombre: this.registerName,
        username: this.registerUsername,
        password: this.registerPassword,
        email: this.registerEmail,
      });
    },
    showLogin() {
      this.ShowLogin = true;
      this.ShowRegister = false;
      this.LoginTabCSS = "nav-link active";
      this.RegisterTabCSS = "nav-link";
    },
    showRegister() {
      this.ShowLogin = false;
      this.ShowRegister = true;
      this.LoginTabCSS = "nav-link";
      this.RegisterTabCSS = "nav-link active";
    },
  },
};
</script>
<style scoped>
.form-content {
  max-width: 300px;
  margin: 0px auto;
}
</style>