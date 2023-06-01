<template>
  <div id="app">
   
    <router-view/>
  </div>
</template>

<style lang="less">
body,html{
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
.selectBox_{
     display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    padding-right: 100px;
}
.searchBox .el-input{
  margin-right: 15px;
}
.searchBox span{
  margin-right: 15px;
}
</style>
