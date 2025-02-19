<template>
  <div class="home">
    <div class="feature-card">
      <!--le dice que esta ruta necesita tt04959 que seria una pelicula en especifico-->
      
      <router-link to="/movie/tt0409591">
        <img src="https://m.media-amazon.com/images/S/pv-target-images/a10716d3a37ff7cbed8a25aae197ec0a8de3afd6bd768c278da0f35eea5112f4._SX1080_FMjpg_.jpg" alt="Naruto Poster" class="featured-img"/>
        <div class="detail">
          <h3>Naruto</h3>
          <p>Naruto es una serie de manga que comenzó en la revista Shōnen Jump en 1998. 
             Pronto se convirtió en un éxito nacional que se adaptó al anime en 2002 y,
             según su sitio oficial, el manga ha vendido hoy más de 250 millones de ejemplares 
             individuales en todo el mundo.</p>
        </div>
      </router-link>
    </div>
    <form @submit.prevent="buscarpelicula()" class="search-box">
       <!--nos ayuda a encapsular elementos de entrada como en este caso el buscar-->
       <!--cada vez que mi usuario escriba en este se va a cambiar la propiedad ademas se pueden utiliza para cualquier tipo de elemento-->
      <input type="text" placeholder="¿Que quieres ver hoy?" v-model="buscar"/>
      <input type="submit" value="buscar">
    </form>

    <div class="movies-list">
      <!--v-for nos permite iterar sobre un array de movies-->
      <div class="movie" v-for = "movie in movies" :key="movie.imdbID">
        
        <!--router link es el encargado de construir la ruta dinamica utilizando el id de la pelicula-->
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <!--v-bind se utiliza para enlazar un atributo de un elemento html con una expresión de vue-->
            <img :src="movie.Poster" alt="Movie Poster"/>
            <div class="type">{{ movie.Type }}</div>
          </div>
          <div class="detalles">
            <p class="year">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
//con el setup, los componentes importados se ponen automaticamente a disposicion de la plantilla
//importamos ref para poder utilizar la reactividad de vue
//Permite que los componenetes en vue se actualicen automaticamente
import {ref} from 'vue';
import env from '@/env.js'; 
//cuando no se utiliza la parte de setup se necesita definir el objeto del componente 
//que vue necesita para crear 
export default {
  setup(){
    //creamos una variable reactiva
    const buscar = ref("");
    const movies = ref([]); 

    const buscarpelicula = ()=>{
      if(buscar.value != ""){
        //la estructura de fetch es una promesa que nos permite hacer peticiones http
        fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${buscar.value}`)
        //nos devuelve una respuesta del servicio tipo json
          .then(responde => responde.json())
          .then(data => {
            movies.value = data.Search;
            buscar.value = "";
            console.log(movies.value);
        });
      }
    }
    return{
      //se retorna para que puedan ser utilizados desde el template 
      buscar,
      movies,
      buscarpelicula
    }
  }
}
</script>

<style>
.home {
  .feature-card {
    position: relative;
  }

  .featured-img {
    display: block;
    width: 100%;
    height: 300px;
    object-fit: cover;
    position: relative;
    z-index: 0;
  }

  .detail {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    padding: 16px;
    z-index: 1;
  }

  .detail h3 {
    color: #fff;
    margin-bottom: 16px;
  }

  .detail p {
    color: #fff;
    
  }
  .search-box{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;
  }

  .search-box input{
    display: block;
    appearance: none;
    border: none;
    outline: none;
    background: none;;
  }

  .search-box input[type="text"]{
    width: 100%;
    color:#fff;
    background-color:#496583;
    font-size: 20px;
    padding: 10px 16px;
    border-radius: 8px;
    margin-bottom: 15px;
    transition: 0.4s;
  }

  .search-box input[type="text"]::placeholder{
    color: #f3f3f3; 
  }

  .search-box input[type="text"]:focus{
    box-shadow: 0px 3px 6px rgba(0,0,0,0.2); 
  }

  .search-box input[type="submit"]{
    width:100%;
    max-width: 300px;
    background-color: #42B883;
    padding: 16px;
    border-radius: 8px;
    color:#fff;
    font-size: 20px;
    text-transform: uppercase;
    transition: 0.4s;
  }

  .search-box input[type="submit"]:active{
    background-color: #3B8070;
  }

  .movies-list {
    display: flex;
    flex-wrap: wrap;
    margin:0;
  }

  .movies-list .movie{
    max-width: 50%;
    flex: 1 1 50%;
    padding: 16px 8px;
  }

  .movies-list .movie .movie-link{
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  .movies-list .movie .product-image{
    position: relative;
    display:block;
  }

  .movies-list .movie .product-image img{
    display: block;
    width: 100%;
    height: 275px;
    object-fit: cover;
  }

  .movies-list .movie .product-image .type{
    position: absolute;
    padding: 8px 16px;
    bottom:16px;
    left:0px;
    background-color: #42B883;
    color: #fff;
    text-transform: capitalize;
  }

  .movies-list .movie .detalles{
    background-color: #496583;
    padding: 16px 8px;
    /*  En esta ropiedad estoy definiendo grow-- capacidad del elemento de crecer
    shrink-- define la capacidad de encogerse, basis-- define el tamaño antes que se aplique el crecimiento o encogimiento */
    flex: 1 1 100%;
    border-radius: 0px 0px 8px 8px;
  }

  .movies-list .movie .detalles .year{
    color: #AAA;
    font-size: 14px;
  }

  h3{
    color: #fff;
    font-weight: 600;
    font-weight: 18px;
    margin: 8px 0px;
  }

}
</style>
