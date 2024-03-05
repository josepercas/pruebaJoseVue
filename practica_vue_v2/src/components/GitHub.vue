<template>
  <div>
    <label for="usuario">Usuario</label>
    <input @keydown.enter="obtenerUsuario()" id="usuario" v-model="login"/>
    <div v-if="existeLogin">
      <img class = "avatar" :src="avatar_url"/>
      <p>{{ login }}</p>
      <a :href="html_url" target="#">Enlace a GitHub del usuario</a>
      <button @click="obtenerRepos()">Repositorios</button>
    </div>
    <div v-else-if="existeLogin==false">
      <p>El usuario {{ login }} no existe</p>
    </div>
    <GitHubRepo v-if="existeRep && !cambioUsuario" :datos=datosRep></GitHubRepo>
    <div v-if="existeRep == false">El usuario {{ login }} no tiene repositorios </div>
    
    

  </div>
</template>

<script>
import GitHubRepo from './GitHubRepo.vue';

export default {
  name: 'GitHub',
  components: {
    GitHubRepo
  },
  props: ['msg'],
  data() {
    return {
      msg3: this.msg,
      url: "https://api.github.com/users/{USER}",
      login: "",
      avatar_url: "",
      html_url: "",
      repos_url: "",
      datos: "",
      existeLogin: NaN,
      datosRep: "",
      existeRep: NaN,
      cambioUsuario: NaN
    }
  },
  methods: {
    //Obtengo los datos del usuario de la API de GitHub
    async obtenerUsuario() {
      if (!this.login == "") {
        let respuesta = await fetch (this.url.replace("{USER}",this.login));

        if (respuesta.ok) {
          this.datos = await respuesta.json();
          
          this.avatar_url = this.datos.avatar_url;
          this.html_url = this.datos.html_url;
          this.login = this.datos.login;
          this.repos_url = this.datos.repos_url;
          this.existeLogin = true;
          this.cambioUsuario = true;
        }
        else {
          this.existeLogin = false;
          this.existeRep = false;
        }
      }
      else {
        alert ("Debes introducir un login no vacío");
      }
    },
    
    //Obtengo los repositorios del usuario
    async obtenerRepos() {

      this.existeRep = false;
      this.cambioUsuario = false;
      let respuesta = await fetch (this.repos_url);
      
      if (respuesta.ok) {
        this.datosRep = await respuesta.json();
        if (this.datosRep.length > 0) {
          this.existeRep = true;
        }
      }

    }
  }
}



</script>



<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
img {
  width: 250px;
  height: 250px;
}
</style>
  