<template>
  <div>
    <Navbar :cookie="cookie" :emailcookie="emailcookie" :nomecookie="nomecookie" />
    <Banner :saldo_dia="saldo_dia" :receita_dia="receita_dia" :despesa_dia="despesa_dia" />
    <div id="nav-date">
      <h1><a @click="subdia()"><img src="/img/setaesquerda.png"></a>&nbsp;&nbsp;&nbsp;
        {{date2}}
      &nbsp;&nbsp;&nbsp;<a @click="adicionardia()"><img src="/img/setadireita.png"></a></h1>
    </div>
    <div class="main-container">
      <Dashboard :transacoes="transacoes" :status="status" :date2="date2" :exist="exist" :loginuser="loginuser"/>
      
    </div>
  </div>
</template>

<script>
import Banner from '../components/Banner'
import Dashboard from '../components/Dashboard.vue'
import Navbar from '../components/Navbar'

export default {
  name: 'Home',
  components: {
    Banner,
    Dashboard,
    Navbar
  },
  emits: ['mostrarMenu'],
  data() {
    return {
      date: new Date(),
      date2: new Date().toLocaleDateString(),
      saldo_dia: 0.0,
      receita_dia: 0.0,
      despesa_dia: 0.0,
      transacoes: null,
      cookie: "",
      nomecookie: "",
      emailcookie: "",
      status: null,
      exist: true,
      loginuser: "",
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
    async adicionardia() {
        var date3 = this.date;
        date3.setDate(date3.getDate()+1);
        this.date2 = new Date(date3).toLocaleDateString();
        this.date = date3;
        this.getLista();
      },
      async subdia() {
        var date3 = this.date;
        date3.setDate(date3.getDate()-1);
        this.date2 = new Date(date3).toLocaleDateString();
        this.date = date3;
        this.getLista();
      },
    async getLista() {

      const req = await fetch('https://teste2.flowcash.app/api/transactions', {
        method: "GET",
        headers: {  "Content-Type" : "application/json",
                    "Authorization" : "Bearer "+this.cookie,
                    "Accept" : "application/json" },
      })

      const data = await req.json()

      if(!data.data) {
        this.exist = false;
      }

      const req2 = await fetch('https://teste2.flowcash.app/api/categories', {
        method: "GET",
        headers: {  "Content-Type" : "application/json",
                    "Authorization" : "Bearer "+this.cookie,
                    "Accept" : "application/json" },
      })

      const data2 = await req2.json()

      this.status = data2.data

      this.transacoes = data.data

      this.calcSaldo();

      this.formatoData();

      this.formatoCat();
    },
    async formatoCat() {
      for(var i in this.transacoes){
        for(var j in this.status){
          if(this.transacoes[i].category_id === this.status[j].id){
            this.transacoes[i].category_id = this.status[j].name;
          }
        }
      }
    },
    async calcSaldo(){
      this.saldo_dia = 0.0;
      this.receita_dia = 0.0;
      this.despesa_dia = 0.0;
      var numero = 0.0;
      for(var i=0; i < this.transacoes.length; i++){
        numero = parseFloat(this.transacoes[i]["value"]);
        if(this.transacoes[i]["type"] === "receita"){
          this.transacoes[i]["type"]
          this.saldo_dia += numero;
          this.receita_dia += numero;
        }
        if(this.transacoes[i]["type"] === "despesa") {
          this.saldo_dia -= numero;
          this.despesa_dia += numero;
        }
      }
    },
    async formatoData(){
      var date;
      var date2;
      for(var i = 0; i<this.transacoes.length; ++i){
      
      date = this.transacoes[i]["date"];
      console.log(date);
      date2 = new Date(date);
      console.log(date2);
      date2.setDate(date2.getDate()+1);
      console.log(date2);
      this.transacoes[i]["date"] = date2.toLocaleDateString();
      console.log(this.transacoes[i]["date"]);
    }
    
    }
  },
  mounted () {
    this.cookie = this.getCookie("loginusuario");
    if(!this.cookie){this.$router.push('/login');}
    this.loginuser = this.cookie;
    this.emailcookie = this.getCookie("emailusuario");
    this.nomecookie = this.getCookie("nomeusuario");
    this.getLista()
  }
}
</script>

<style scoped>
  a {
    cursor: pointer;
  }

  #nav-date {
    margin-top: 25px;
  }
  #nav-date h1 {
    font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    text-align: center;
    font-size: 36px;
    color: #222;
  }
  
  #nav-date a {
    color: #000;
    align-items: baseline;
    text-decoration: none;
    margin: 12px;
    transition: .5s;
  }

  #nav-date a:hover {
    color: #fcba03;
  }
</style>