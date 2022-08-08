<template>
  <div>
    <Navbarlimpo/>
    <div class="main-container">
      <center><h1>Nova transação:</h1></center>
      <BurgerForm :login_usuario="login_usuario"/>
    </div>
  </div>
</template>

<script>
import BurgerForm from '../components/BurgerForm'
import Navbarlimpo from '../components/Navbarlimpo.vue'

export default {
  name: 'Transacao',
  components: {
    BurgerForm,
    Navbarlimpo
  },
  data() {
    return {
      login_usuario: null

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
    }
  }
}
</script>
