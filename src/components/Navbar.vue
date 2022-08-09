<template>
  <div style="width: 100%">
    <div id="nav">
      <div style="width: 100%">
        <div style="width: 50%">
          <div v-if="cookie">
            <button @click="apagaCookie()">SAIR</button>
            <div><p class="bem-vindo">&nbsp;&nbsp;Bem-vindo,&nbsp;{{nomecookie}}<br>&nbsp;&nbsp;{{emailcookie}}</p></div>
          </div>
          <div v-else>
            <router-link to="/login">
            <button class="login-btn"  @click="this.mostrar_btn = !this.mostrar_btn">LOGIN</button>
            </router-link></div>
        </div>
        <div style="width: 50%; justify-content: flex-end;">
          <router-link to="/transacao"><button v-show="cookie">ADICIONAR TRANSAÇÃO</button></router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: "Navbar",
  data () {
    return{
      logado: false,
      nome: "Ronildo",
    }
  },
  props: {
    cookie: String,
    emailcookie: String,
    nomecookie: String
  },
  methods: {
    apagaCookie() {
      document.cookie = "loginusuario = ; Path=/; Expires=Thu, 01 Jan 1970 00:00:01 GMT;";
      document.cookie = "emailusuario = ; Path=/; Expires=Thu, 01 Jan 1970 00:00:01 GMT;";
      document.cookie = "idusuario = ; Path=/; Expires=Thu, 01 Jan 1970 00:00:01 GMT;";
      document.cookie = "nomeusuario = ; Path=/; Expires=Thu, 01 Jan 1970 00:00:01 GMT;";
      location.reload();
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

  .bem-vindo {
    padding-top: 5px;
    font-size:15px;
    font-weight: bold;
    color: #333;
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