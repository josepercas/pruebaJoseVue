<template>
  <div class = "filtro">
    <label for="id">Id inical</label>
    <input type="text" name="id" id="id" @keydown.enter="obtenerTodosDatos()" v-model="idFiltro">  
    <label for="idF">Id final</label>
    <input type="text" name="idF" id="idF" @keydown.enter="obtenerTodosDatos()" v-model="idFiltroF">  

    <button @click="filtrar=!filtrar"><a v-if="filtrar">Ocultar filtro</a><a v-else>Mostrar filtro</a></button>
    <button @click="editar=!editar"><a v-if="editar">Ocultar edición</a><a v-else>Mostrar edición</a></button>
    <button @click="modificar=!modificar"><a v-if="modificar">Ocultar modificar PUT</a><a v-else>Mostrar modificar PUT</a></button>
    <button @click="modificarPatch=!modificarPatch"><a v-if="modificarPatch">Ocultar modificar PATCH</a><a v-else>Mostrar modificar PATCH</a></button>
    <!--DIV para realizar filtros de datos-->
    <div v-if="filtrar">
      <table>
        <th>Id</th><th>userId</th><th>Title</th><th>Body</th>
        <tr v-for="item in datos">
          <td v-if="parseInt(item.id)>=idFiltro && parseInt(item.id)<=idFiltroF && item.id!=''"><input type="checkbox"> {{ item.id }}</td>
          <td v-if="parseInt(item.id)>=idFiltro && parseInt(item.id)<=idFiltroF && item.id!=''">{{ item.userId}}</td>          
          <td v-if="parseInt(item.id)>=idFiltro && parseInt(item.id)<=idFiltroF && item.id!=''">{{ item.title }}</td>
          <td v-if="parseInt(item.id)>=idFiltro && parseInt(item.id)<=idFiltroF && item.id!=''">{{ item.body }} <button @click="borroRegistro(item.id)">Borrar</button></td>
          
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

  <!--FORM para guardar nueva información-->
  <div v-if="editar">
    <h2>Crear nuevo registro</h2>
    <form @submit.prevent="guardoDatos()">
      <label for="userId">User Id</label>
      <input type="text" name="userId" id="userId" v-model="datosUsar.userId">
      <label for="title">Title</label>
      <input type="text" name="tile" id="title" v-model="datosUsar.title">
      <label for="body">Body</label>
      <input type="text" name="body" id="body" v-model="datosUsar.body">
      <button @submit.prevent="guardoDatos()">Añadir datos</button>
    </form>
  </div>

  <!--FORM para modificar información-->
  <div v-if="modificar" >
    <h2>Modificar registro PUT</h2>
    <form @submit.prevent="modificoDatos()">
      <label for="id">Id</label>
      <input @keydown.tab="obtenerDato()" type="text" name="id" id="id" v-model="datosUsar.id">
      <label for="userId">User Id</label>
      <input type="text" name="userId" id="userId" v-model="datosUsar.userId">
      <label for="title">Title</label>
      <input type="text" name="tile" id="title" v-model="datosUsar.title">
      <label for="body">Body</label>
      <input type="text" name="body" id="body" v-model="datosUsar.body">
      <button @submit.prevent="modificoDatos()">Modifico datos</button>
    </form>
  </div>

  <!--FORM para modificar información-->
  <div v-if="modificarPatch" >
    <h2>Modificar registro PATCH</h2>
    <form @submit.prevent="modificoDatosPatch()">
      <label for="id">Id</label>
      <input type="text" name="id" id="id" v-model="datosUsarPatch.id">
      <label for="userId">User Id</label>
      <input type="text" name="userId" id="userId" v-model="datosUsarPatch.userId">
      <label for="title">Title</label>
      <input type="text" name="tile" id="title" v-model="datosUsarPatch.title">
      <label for="body">Body</label>
      <input type="text" name="body" id="body" v-model="datosUsarPatch.body">
      <button @submit.prevent="modificoDatosPatch()">Modifico datos PATCH</button>
    </form>
  </div>

</template>



<script>

let newDatos = {
  id: 0,
  title: "",
  body: "",
  userId: 0,
}

let newDatosPatch = {
  id: 0,
  title: "",
  body: "",
  userId: 0,
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
      filtrar: false,
      modificar: false,
      modificarPatch: false, 
      resultadoGuardar: false,
      datosAux: "",
      datosUsar: newDatos,
      datosUsarPatch: newDatosPatch,
      checado: false
    }
  },
  methods: {
    
    //Obtengo un dato partiendo del índice. Uso GET (es la opción por defecto por lo que no hay que indicarla)
    async obtenerDato() {
      let respuesta = await fetch (this.url + this.datosUsar.id);

      if(respuesta.ok) {
        this.datosUsar = await respuesta.json();
        this.newDatos = this.datosUsar;
      }
      else {
        alert("No existe");
      }
    },

    //Obtengo todos los datos. Uso GET (es la opción por defecto por lo que no hay que indicarla)
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

    //Guardo nuevos datos. Uso POST.
    async guardoDatos() {
      this.resultadoGuardar = false;
      let respuesta = await fetch (this.url, {
        method: 'POST',
        headers: {'Content-Type': 'application/json;charset=utf-8'},
        body: JSON.stringify(newDatos)
      });

      if (respuesta.ok) {
        this.datosAux = await respuesta.json();
        this.resultadoGuardar = true;
        console.log(this.datosAux);
      }
      else {
        console.log("No se ha podido guardar");
      }

    },

    //Modifico los datos usando PUT. Realmente con PUT se modifica todo el registro aunque no se cambie nada.
    async modificoDatos() {
      newDatos = this.datosUsar;
      let respuesta = await fetch (this.url  + newDatos.id, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json;charset=utf-8'
        },
        body: JSON.stringify(newDatos)
        
      });

      if (respuesta.ok) {
        this.datosAux = await respuesta.json();
        console.log(this.datosAux);
      }
      else {
        console.log("No existe el registro a modificar.");
      }
    },

    //Modifico lalgunos datos del registro, por lo que debo usar PATCH.
    async modificoDatosPatch() {

      let datoObj = {};

      for (let item in newDatosPatch) {
        console.log(`Clave: ${item} Valor: ${newDatosPatch[item]} `);
        if (newDatosPatch[item] != "") {
          datoObj[`${item}`] = newDatosPatch[item];
        }
      }
      console.log("Id que tengo al principio: " + newDatosPatch.id);
      let respuesta = await fetch (this.url + newDatosPatch.id, {
        method: 'PATCH',
        headers: {
            'Content-Type': 'application/json;charset=utf-8'
          },
          body: JSON.stringify(datoObj)
      });

      if(respuesta.ok) {
        this.datosAux = await respuesta.json();
        console.log(this.datosAux);
      }
      else {
        console.log ("No existe el registro a modificar.")
      }
    },

    async borroRegistro(idBorrar) {
        let respuesta = await fetch (this.url + idBorrar, {
          method: 'DELETE'
        });

        if(respuesta.ok) {
          let datos = await respuesta.json();
          alert (idBorrar);
        }
        else {
          alert (`No existe el registro a borrar ${idBorrar}`);
        }
    }
  }

}
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
