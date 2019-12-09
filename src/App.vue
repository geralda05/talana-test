<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <Burger></Burger>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <button data-toggle="modal" data-target="#exampleModal" class="ml-auto btn-success btn"><i class="fa fa-shopping-cart"></i></button>
      </div>
    </nav>
    <div class="row bg-dark" style="min-height:100vh">
      <Sidebar class="col-12 col-sm-6 col-md-3 bg-info p-3">
        <ul class="list-group pl-3">
          <li class="list-group-item active">Categorias</li>
          <li style="cursor:pointer;" v-for="categoria in categorias" :key="categoria.id" v-on:click="filterItems(categoria)" class="list-group-item">{{categoria}}</li>
        </ul>
      </Sidebar>
      <div class="w-100 col-12 col-sm">
        <div class="w-100 bg-dark p-3">
                <h4 class="text-white"><span v-if="showProductos.length == 0">No hay productos con la categoria de </span>{{currentCategoria}}</h4>
        <div class="row justify-content-center" v-if="showProductos.length > 0">
          <div class="col-12 col-sm-6 col-md-4 col-lg-3 d-flex" style="max-width:300px;" v-for="producto in showProductos" :key="producto.id">
            <div class="card w-100">
              <img v-bind:src="producto.photo" class="card-img-top" />
              <div class="card-body">
                <h5 class="card-title">{{producto.name}}</h5>
                <p class="card-subtitle mb-2 text-muted">          {{producto.category.name}}</p>
              </div>
              <div class="card-footer text-muted">
                <form class="ml-auto w-100 text-right" style="display:inline-block" v-on:submit.prevent="addToCart(producto)">
                  <span class="d-inline-block m-0 p-0">${{producto.price}}</span>
                  <span class="d-inline-block ml-3" style="font-size:10px; margin-right:4px">Cantidad</span>
                  <input type="number" class="form-control" v-model="producto.cantidad" required style="display:inline-block !important; width: 55px" />
                  <button class="d-inline-block btn btn-success" type="submit"><i class="fa fa-plus"></i></button>
                </form>
              </div>  
            </div>
          </div>
        </div>
        </div>
      </div>
    </div>
    <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered modal-lg" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Shopping Cart</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <h1 class="text-center" v-if="shoppingCart.length == 0">No hay productos en el carrito de compras. Añada uno.</h1>
            <table class="table table-striped" v-if="shoppingCart.length > 0">
              <thead>
                <tr>
                  <th scope="col">Producto</th>
                  <th scope="col">Categoria</th>
                  <th scope="col">Cantidad ordenada</th>
                  <th scope="col">Precio</th>
                  <th scope="col">Subtotal</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="producto in shoppingCart" :key="producto.id">
                  <th scope="row">{{producto.name}}</th>
                  <td>{{producto.category.name}}</td>
                  <td>{{producto.cantidad}}</td>
                  <td>${{producto.price}}</td>
                  <td>${{producto.price * producto.cantidad}}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
 </div>
</template>

<script>
import Burger from './components/Burguer.vue';
import Sidebar from './components/Sidebar.vue';
import Swal from 'sweetalert2'
 import axios from 'axios'

export default {
 name: 'app',
 components: {
   Burger,
   Sidebar
 },
 data(){
   return{
    productos:[],
    categorias:['Categoria 1','Categoria 2', 'Categoria 3', 'Alimentos', 'Collares'],
    showProductos:[],
    shoppingCart:[],
    currentCategoria:''
  }
 },
 methods:{
   addToCart(article){

     this.shoppingCart.push(article)
      Swal.fire(
        'Enhorabuena',
        'Se añadio el producto al carrito de compras',
        'success'
      )
   },
   filterItems(v){
    // alert(v)
    this.currentCategoria = v
     this.showProductos = [];
     let contador = 0;
     do{
       if(contador == 0){
         this.showProductos = []
       }
       if(this.productos[contador].category.name == v){
         this.showProductos.push(this.productos[contador])
       }
       contador++
     }while(contador < this.productos.length)
   }
 },
 mounted(){
   const self = this;
   axios.get('http://sva.talana.com:8000/api/product/', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    self.productos = response.data
   self.showProductos = response.data
  })

 }
}
</script>
<style>
html {
   height: 100%;
 }


 .main-nav {
   display: flex;
   justify-content: space-between;
   padding: 0.5rem 0.8rem;
 }

 ul.sidebar-panel-nav {
   list-style-type: none;
 }

 ul.sidebar-panel-nav > li > a {
   text-decoration: none;
   font-size: 1.5rem;
   display: block;
   padding-bottom: 0.5em;
 }
</style>