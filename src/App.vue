<template>
  <div id="app">
    <div class="row justify-content-center">
      <div class="col-sm-8">
        <div class="hello">
          <h1>Estudiantes</h1>
        </div>
        <div class="form-group">
          <input type="text" v-model="estudiante.nombre" ref="nombre" class="form-control" v-on:keyup.enter="addItem">
        </div>
        <div class="table-responsive">
          <table class="table">
            <tr>
              <td>NOMBRE</td>
              <td>EDITAR</td>
              <td>ELIMINAR</td>
            </tr>
            <tr v-for="item in todos" :key="item.id">
              <td>{{item.nombre}}</td>
              <td><button type="button" class="btn btn-secondary btn-sm" @click="updateItem(item.id)">EDITAR</button></td>
              <td><button type="button" class="btn btn-secondary btn-sm" @click="deleteItem(item.id)">ELIMINAR</button></td>
            </tr>
          </table>
        </div>      
      </div>
    </div>
  </div>
</template>

<script>

import config from './config'
import firebase from 'firebase/app'
import 'firebase/firestore'

const db = firebase.initializeApp(config).firestore().collection('estudiantes')

export default {
  name: 'App',
  firestore: {
    todos: db,
  },
  data() { 
    return {
      todos: [],
      estudiante:{
        nombre: ''
      }
    } 
  },
  methods:{
    addItem() {
      db.add(this.estudiante);
      this.estudiante.nombre = ''
      this.$refs.nombre.focus();
    },
    deleteItem(id){
      db.doc(id).delete();
    },
    updateItem(id){
      db.doc(id).update(this.estudiante);
      this.estudiante.nombre = ''
      this.$refs.nombre.focus();
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
