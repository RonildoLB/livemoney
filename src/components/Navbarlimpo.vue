<template>
  <div style="width: 100%">
    <div id="nav">
      
    </div>
  </div>
</template>

<script>

export default {
  name: "Navbarlimpo",
  data () {
    return{
    }
  }
}
</script>

<style scoped>
  div {
    display: flex;
  }

   #nav {
    background-color: #fcce03;
    border-bottom: 1px solid #fce303;
    padding: 15px 50px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    width: 100%;
  }

  .login-btn {
    justify-self: flex-start;
  }

  #nav button {
    background-color: #222;
    border: none;
    color: #ffffff;
    padding: 8px 12px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 16px;
    font-weight: bold;
    margin: 4px 2px;
    transition-duration: 0.4s;
    cursor: pointer;
    border-radius: 6px;
  }

  #nav button:hover {
    background-color: #444;
  }
</style>