<template>
  <div>
  <Message :msg="msg" v-show="msg" />
  <div>
    <form id="login-form" method="POST" @submit="loginUser">
        <div class="input-container" >
        <label for="valor">Digite seu nome:</label>
        <input type="text" name="name" id="name" v-model="name" placeholder="Digite seu nome" required="required">
      </div>
      <div class="input-container" >
        <label for="valor">Digite seu E-mail:</label>
        <input type="text" name="email" id="email" v-model="email" placeholder="Digite seu E-mail" required="required">
      </div>
      <div class="input-container" >
        <label for="valor">Digite sua senha:</label>
        <input type="password" name="password" id="password" v-model="password" placeholder="Digite sua senha" required="required">
      </div>
      <div class="button-container">
        <input class="submit-btn" type="submit" value="Cadastrar">
        <router-link to="/"><input class="submit-btn" type="button" value="Cancelar"></router-link>
      </div>
    </form>
  </div>
  </div>
</template>

<script>
import Message from './Message'

export default {
  name: "LoginForm",
  data() {
    return {
        name: null,
        email: null,
        password: null,
        msg: null,
    }
  },
  methods: {
    async loginUser(e) {

      e.preventDefault()

      const data = {
        name: this.name,
        email: this.email,
        password: this.password,
      }

      this.msg = "Processando..."

      const dataJson = JSON.stringify(data)

      const req = await fetch("https://teste2.flowcash.app/api/register", {
        method: "POST",
        headers: { "Content-Type" : "application/json" },
        body: dataJson
      });

      const res = await req.json()

      var texto = JSON.stringify(res);

      this.nozes = texto

      if(texto.indexOf("The email has already been taken.") != -1){
        this.msg = "Não foi possível cadastrar. E-mail já cadastrado em nosso banco de dados. Faça login ou tente novamente com um e-mail diferente."
        setTimeout(() => this.msg = "", 10000)
      }
      else{
        this.msg = "Usuário Cadastrado."

        // clear message
        setTimeout(() => this.msg = "", 3000);

        setTimeout(()=>{ this.$router.push('/');}, 3000)

        // limpar campos
        this.name = ""
        this.email = ""
        this.password = ""
      }

      
      
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