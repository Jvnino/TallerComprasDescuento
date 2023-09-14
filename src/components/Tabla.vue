<script setup lang="ts">
import { defineProps, defineEmits } from "vue";

//Variables
const {productos}= defineProps(["productos"])
const emits= defineEmits(["eliminar"])

//Funcion eliminar
function borrar(i: number){
    emits("eliminar",i)
}
</script>

<template>
    <table>
        <thead>
            <tr>
                <th>Id</th>
                <th>Articulo</th>
                <th>Cantidad</th>
                <th>Valor unitario</th>
                <th>Total</th>
                <th>Eliminar</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(producto, index) of productos" :key="index">
                <td>{{ index }}</td>
                <td>{{ producto.articulo }}</td>
                <td>{{ producto.cantidad }}</td>
                <td> {{ producto.vUnitario }}</td>
                <td> {{ producto.cantidad * producto.vUnitario }}</td>
                <td> <button v-on:click="borrar(index)">🗑️</button></td>
            </tr>
        </tbody>
    </table>
</template>

<style>
</style>