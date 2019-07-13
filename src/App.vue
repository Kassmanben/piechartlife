<template>
  <div id="app">
    <div id="nav" class="header">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.header a {
  font-size: x-large;
  text-decoration: none;
  padding: 20px;
  color: #2c3e50
}

.header a:hover{
  text-decoration:underline
}

.header a:visited{
  color: #2c3e50;
}
</style>
