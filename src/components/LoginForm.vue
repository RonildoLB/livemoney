<template>
  <div>
  <div id="nav"></div>
  <Message :msg="msg" v-show="msg" />
  <div>
    <form id="login-form" method="POST" @submit="loginUser">
      <div class="input-container" >
        <label for="valor">Digite seu E-mail:</label>
        <input type="text" name="email" id="email" v-model="email" placeholder="Digite seu E-mail">
      </div>
      <div class="input-container" >
        <label for="valor">Digite sua senha:</label>
        <input type="password" name="password" id="password" v-model="password" placeholder="Digite sua senha">
      </div>
      <div class="button-container">
        <input class="submit-btn" type="submit" value="Login">
        <router-link to="/"><input class="submit-btn" type="button" value="Cancelar"></router-link>
      </div>
    </form>
    {{conversa}}
  </div>
  </div>
</template>

<script>
import Message from './Message'

export default {
  name: "LoginForm",
  data() {
    return {
      email: null,
      password: null,
      msg: null,
      conversa: null
    }
  },
  methods: {
    async loginUser(e) {

      e.preventDefault()

      const data = {
        email: this.email,
        password: this.password,
      }

      this.msg = "Processando..."

      const dataJson = JSON.stringify(data)

      const req = await fetch("https://teste2.flowcash.app/api/login", {
        method: "POST",
        headers: { "Content-Type" : "application/json" },
        body: dataJson
      }).catch(response => {
        this.msg = "Erro.";
        setTimeout(() => this.msg = "", 5000);
      });

      const res = await req.json()

      document.cookie = "emailusuario = "+this.email+"; path='/'";
       document.cookie = "loginusuario = "+res.token+"; path='/'";


       const req2 = await fetch('https://teste2.flowcash.app/api/user', {
        method: "GET",
        headers: {  "Content-Type" : "application/json",
                    "Authorization" : "Bearer "+res.token,
                    "Accept" : "application/json" },
      })

      const data2 = await req2.json()

      console.log(data2)

       document.cookie = "idusuario = "+data2.id+"; path='/'";
       document.cookie = "nomeusuario = "+data2.name+"; path='/'";

        this.msg = "Login.";
        setTimeout(() => this.msg = "", 1000);
        setTimeout(()=>{ this.$router.push('/');}, 1000);
        // limpar campos
        this.email = ""
        this.password = ""
      
    }
  },
  mounted () {
  },
  components: {
    Message
  }
}
</script>

<style scoped>
  #login-form {
    max-width: 380px;
    margin: 0px auto;
    margin-top: 25px;
  }

  .input-container {
    display:flex;
    flex-direction: column;
    margin-bottom: 20px;
  }

  .button-container {
    display:inline-block;
    flex-direction: row;
    margin: 0px 10px 10px 10px;

  }

  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
  }

  input, select {
    padding: 5px 10px;
  }

  #opcionais-container {
    flex-direction: row;
    flex-wrap: wrap;
  }

  #opcionais-title {
    width: 100%;
  }

  .checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
  }

  .checkbox-container span,
  .checkbox-container input {
    width: auto;
    margin-right: 15px;
  }

  .checkbox-container span {
    font-weight: bold;
  }

  .submit-btn {
    background-color: #222;
    color:#fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    cursor: pointer;
    transition: .5s;
    margin: 0px 10px 10px 10px;
    border-radius: 6px;
  }

  .submit-btn:hover {
    background-color: transparent;
    color: #222;
  }
</style>