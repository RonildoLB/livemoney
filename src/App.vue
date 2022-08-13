<template>
  <div>
    <router-view></router-view>
  </div>
</template>
<script>

export default {
  data: function() {
    return {
    }
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