<script setup>
//setup es una nueva forma de escribir los componentes en vue 3
</script>

<template>
  <header>
    <router-link to="/">
      <h1><span>Vue</span>Movies</h1>
    </router-link> 
  </header>
  <main>
    <router-view />
  </main>
</template>

<style>
/* el scoped se utiliza para aplicar un estilo a un componente único */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Fira Sans', sans-serif;
}

/* Se utiliza para aplicar estilos a los elementos seleccionados */
*::selection {
  background: rgba(66, 184, 131, 0.5); /* #42B883 con 50% de opacidad */
}

body {
  background-color: #35495E;
}

a {
  text-decoration: none;
  color: #42B883;
}

header {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px 16px;
  background-color: #2C3D4E;
  box-shadow: 0px 0px 6px rgba(0, 0, 0, 0.1);
}

h1 {
  color: #FFF;
  font-size: 28px;
}

span {
  color: #42B883;
}
</style>