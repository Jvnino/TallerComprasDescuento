<script setup lang="ts">
import { reactive, ref } from "vue";
import Tabla from "./components/Tabla.vue"

type Productos= {articulo: string, cantidad: number, vUnitario: number}

// Variables
const articulo= ref("")
const cantidad= ref(0)
const vUnitario= ref(0)
const productos= reactive<Productos[]>([])
const descuento= ref(0)
const total= ref(0)
const totalFinal=ref(0)
const cantidadTotal= ref(0)

// Funcion agregar
function agregar(e: Event) {
  e.preventDefault()
  productos.push({
    articulo: articulo.value, 
    cantidad: cantidad.value, 
    vUnitario: vUnitario.value
  })
  calcular()
  rebajas()
  calcularRebajas()
}
// Funcion calcular precio neto
function calcular(){
  total.value=0
  totalFinal.value=0
  cantidadTotal.value=0
  descuento.value=0

  for (let index = 0; index < productos.length; index++) {
    total.value = total.value + (productos[index].vUnitario * productos[index].cantidad)
  }
}
// Funcion calcular los descuentos
function rebajas(){

  //Descuento por valor
  if (total.value >= 60000) {
    descuento.value= 5
  }
  if (total.value >= 120000) {
    descuento.value= 10
  } 
  if (total.value >= 240000) {
    descuento.value= 15
  }

  //Descuento por cantidad
  for (let index = 0; index < productos.length; index++) {
    cantidadTotal.value= cantidadTotal.value + (productos[index].cantidad)
  }
  if (cantidadTotal.value >=6) {
    descuento.value= 10
  }
  if (cantidadTotal.value >=12) {
    descuento.value= 20
  }
}
// Funcion calcular precio final
function calcularRebajas(){
  totalFinal.value= total.value - (total.value * descuento.value/100)
}
// Funcion eliminar fila
function eliminar(index: number){
  productos.splice(index, 1)
  calcular()
  rebajas()
  calcularRebajas()
}

</script>

<template>
  <div>
    <!-- Seccion que contiene formulario -->
    <section>
      <form v-on:submit="agregar">
        <!-- Input articulo -->
        <label for="articulo">Articulo</label>
        <input id="articulo" type="text" v-model="articulo">

        <!-- Input cantidad -->
        <label for="cantidad">Cantidad</label>
        <input id="cantidad" type="text" v-model="cantidad">

        <!-- Input Valor unitario -->
        <label for="vUnitario">Valor unitario</label>
        <input id="vUnitario" type="text" v-model="vUnitario">

        <!-- Boton -->
        <button type="submit">Agregar</button>
      </form>
    </section>


    <!-- Seccion que contiene la tabla -->
    <section>
      <Tabla :productos="productos" v-on:eliminar="eliminar"/>

      <!-- Seccion que contiene los valores finales -->
     <section>
        <p>Valor inicial: {{ total }}</p>
        <p> Descuento: {{ descuento }}%</p>
        <p class="text-xl">Valor total: {{ totalFinal }}</p>
      </section>
    </section>
  </div>
</template>

<style scoped>
div{
  display: grid;
  grid-template-columns: 1fr 2fr;
  grid-template-rows: 1fr 2fr;
}
/* Estilo para el formulario */
form {
  max-width: 400px;
  margin: 20px;
  padding: 20px;
  background-color: #ffdfef;
  border-radius: 5px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  text-align: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  justify-content: center;
}

label {
  display: block;
  margin-bottom: 5px;
}

input[type="text"] {
  width: 100%;
  padding: 10px;
  margin-bottom: 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button[type="submit"] {
  background-color: #bd497f;
  color: #ffffff;
  padding: 10px 20px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
}

button[type="submit"]:hover {
  background-color: #661239;
}

/* Estilo para la tabla */
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  justify-content: center;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  border-radius: 5px;
}

table, th, td {
  background-color: #ffdfef;
}

th, td {
  padding: 10px;
  text-align: center;
}

/* Estilo para los valores finales */
.text-xl {
  font-size: 24px;
  font-weight: bold;
}

/* Estilo para la sección que contiene los valores finales */
section {
  margin-top: 20px;
  margin-right: 20px;
  text-align: center;
}

p{
  text-align: end;
  margin-right: 50px;
}

</style>
