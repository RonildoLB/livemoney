<template>
  <div>
    <Navbarlimpo />
    <div class="main-container">
      <center><h1>Login:</h1></center>
      <LoginForm />
      <center>Não possui uma conta? <router-link to="/cadastro"><a href="">Cadastre-se</a></router-link></center>
    </div>
  </div>
</template>

<script>
import LoginForm from '../components/LoginForm'
import Navbarlimpo from '../components/Navbarlimpo.vue'

export default {
  name: 'Login',
  components: {
    LoginForm,
    Navbarlimpo
  }
}
</script>