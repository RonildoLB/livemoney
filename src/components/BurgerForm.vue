<template>
  <div>
    
  <Message :msg="msg" v-show="msg" />
  <div>
    <form id="burger-form" method="POST" @submit="createTransac">
      <div class="input-container" >
        <label for="nome">Descrição:</label>
        <input type="text" id="description" name="description" v-model="description" placeholder="Digite uma breve descrição" required="required">
      </div>
      <div class="input-container" >
        <label for="valor">Digite o valor (R$):</label>
        <input type="number" step=0.01 name="value" id="value" v-model="value" placeholder="Digite o valor" required="required">
      </div>
      <div class="input-container" >
        <label for="categorias">Categoria:</label>
        <select v-if="criacategoria" name="categoria" id="categoria" v-model="category_id" @change="newCat($event)" required="required">
          <option value="Criar categoria">Criar categoria</option>
          <option v-for="categoria in categories" :key="categoria.id" :value="categoria.id">{{ categoria.name }}</option>
        </select>
        <input v-else type="text" name="categoriat" id="categoriat" v-model="categoria_name" placeholder="Nome da categoria" required="required">
      </div>
      <div class="input-container" >
        <label for="data">Digite a data:</label>
        <input type="text" name="date" id="date" v-model="datas" placeholder="Digite a data" required="required">
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais">Tipo:</label>
        <div class="checkbox-container" required="required">
          <input type="radio" name="receita" v-model="type" value="receita"><span>Receita</span>
          <input type="radio" name="despesa" v-model="type" value="despesa"><span>Despesa</span>
        </div>
      </div>
      <div class="button-container">
        <input class="submit-btn" type="submit" value="Adicionar">
        <router-link to="/"><input class="submit-btn" type="button" value="Cancelar"></router-link>
      </div>
    </form>
    {{criacategoria}}
  </div>
  </div>
</template>

<script>
import Message from './Message'

export default {
  name: "BurgerForm",
  data() {
    return {
      loginusuario: null,
      id_usuario: null,
      description: null,
      value: null,
      type: null,
      datas: new Date().toLocaleDateString(),
      date: null,
      categories: null,
      category_id: null,
      categoria_name: null,
      name: null,
      status: "0",
      msg: null,
      criacategoria: 1
    }
  },
  methods: {
    getCookie(nome) {
      var nomeCookie = nome + "=";
      var ca = document.cookie.split(';');
      for(var i=0;i<ca.length;i++){
        var c = ca[i];
        while (c.charAt(0)==' ') c = c.substring(1,c.length);
        if(c.indexOf(nomeCookie) == 0) return c.substring(nomeCookie.length, c.length);
      }
      return null;
    },
    async getCat() {
      this.loginusuario = this.getCookie("loginusuario");
      this.id_usuario = this.getCookie("idusuario");

      const req2 = await fetch('https://teste2.flowcash.app/api/categories', {
        method: "GET",
        headers: {  "Content-Type" : "application/json",
                    "Authorization" : "Bearer "+this.loginusuario,
                    "Accept" : "application/json" },
      })

      const data2 = await req2.json()

      this.categories = data2.data
    },
    async createTransac(e) {

      e.preventDefault()

      if(this.criacategoria === 0) {
        const data2 = {
          name: this.categoria_name
        }

        const dataJson2 = JSON.stringify(data2)    


        const req2 = await fetch('https://teste2.flowcash.app/api/categories', {
          method: "POST",
          headers: {  "Content-Type" : "application/json",
                      "Authorization" : "Bearer "+this.loginusuario,
                      "Accept" : "application/json" },
          body: dataJson2
        })

        const resp = await req2.json()
        console.log(resp)

        this.category_id = resp.data.id
        console.log(resp.data.id)
      }

      var data4 = new Date(this.datas);

      console.log(data4);

      const data = {
        user_id: this.id_usuario,
        category_id: this.category_id,
        description: this.description,
        date: data4,
        status: "0",
        type: this.type,
        value: this.value 
      }

      const dataJson = JSON.stringify(data)    


      const req = await fetch('https://teste2.flowcash.app/api/transactions', {
        method: "POST",
        headers: {  "Content-Type" : "application/json",
                    "Authorization" : "Bearer "+this.loginusuario,
                    "Accept" : "application/json" },
        body: dataJson
      })

      const res = await req.json()

      console.log(res)

      this.msg = "Transação adicionada."

      // clear message
      setTimeout(() => this.msg = "", 3000)

      // limpar campos
      this.description = ""
      this.value = ""
      this.datas = ""
      this.category_id = ""
      this.category_id = ""
      this.type = ""
      
    },
    async newCat(event) {
      if (event.target.value === "Criar categoria"){
        this.criacategoria = 0;
        this.categoria = "";
      };

    }
  },
  mounted () {
    this.getCat()
  },
  components: {
    Message
  },
}
</script>

<style scoped>
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  #burger-form {
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