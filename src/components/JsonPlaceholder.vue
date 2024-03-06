<template>
  <div class = "filtro">
    <label for="id">Id inical</label>
    <input type="text" name="id" id="id" @keydown.enter="obtenerTodosDatos()" v-model="idFiltro">  
    <label for="idF">Id final</label>
    <input type="text" name="idF" id="idF" @keydown.enter="obtenerTodosDatos()" v-model="idFiltroF">  

    <button @click="guardoDatos()">Editar</button>
    <div>
      <table>
        <th>userId</th><th>Id</th><th>Title</th><th>Body</th>
        <tr v-for="item in datos">
          <td v-if="parseInt(item.userId)>=idFiltro && parseInt(item.userId)<=idFiltroF && item.userId!=''">{{ item.userId}}</td>
          <td v-if="parseInt(item.userId)>=idFiltro && parseInt(item.userId)<=idFiltroF && item.userId!=''">{{ item.id }}</td>
          <td v-if="parseInt(item.userId)>=idFiltro && parseInt(item.userId)<=idFiltroF && item.userId!=''">{{ item.title }}</td>
          <td v-if="parseInt(item.userId)>=idFiltro && parseInt(item.userId)<=idFiltroF && item.userId!=''">{{ item.body }}</td>
        </tr>
        <tr v-if="resultadoGuardar">
          <td>{{ datosGuardar.userId}}</td>
          <td>{{ datosGuardar.id }}</td>
          <td>{{ datosGuardar.title }}</td>
          <td>{{ datosGuardar.body }}</td>
        </tr>

      </table>
    </div>
  </div>

  

  <div v-if="editar">
    <label for></label>
    <input type="text" name="id" id="id" @keydown.enter="obtenerDatos()" v-model="idFiltro">  

  </div>

</template>

<script>

let newDatos = {
  title: "Historias descabelladas",
  body: "Trata sobre auténticas historias mentales, realizadas con la mente. Lo que alguien haría pero nunca hace.",
  userId: 45,
}

export default {
  name: 'JsonPlaceholder',
  props: ['msg'],
  data() {
    return {
      url: "https://jsonplaceholder.typicode.com/posts/",
      datos: "",
      idFiltro: 0,
      idFiltroF: 0,
      existe: false,
      editar: false, 
      resultadoGuardar: false,
      datosGuardar: "",
    }
  },
  methods: {

    async obtenerTodosDatos() {
      let respuesta = await fetch (this.url);

      if(respuesta.ok) {
        this.datos = await respuesta.json();
        this.existe = true;
      }
      else {
        this.existe = false;
        alert("No existe");
      }
    },

    async guardoDatos() {
      this.resultadoGuardar = false;
      let respuesta = await fetch (this.url, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json;charset=utf-8'
        },
        body: JSON.stringify(newDatos)
      });

      if (respuesta.ok) {
        this.idFiltro = 0;
        this.idFiltroF = 0;
        this.datosGuardar = await respuesta.json();
        this.resultadoGuardar = true;
        console.log(this.datosGuardar);
      }
      else {
        console.log("No se ha podido guardar");
      }

    }
  }


}
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
