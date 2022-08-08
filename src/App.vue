<template>
  <div>
    <div>
      <router-view></router-view>
    </div>
  </div>
</template>
<script>
import Navbar from './components/Navbar'

export default {
  data: function() {
    return {
    }
  },
  components: {
    Navbar,
  },
  methods: {
  },
  mounted () {
  }
}
</script>

<style>
  * {
    font-family: Helvetica;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  .main-container {
    margin: 25px 50px 50px 50px;
    min-height: 250px;
  }
</style>