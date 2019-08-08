<template>
  <div id="app" class="container">
    <div class="col-12 col-md-8 col-lg-8">
      <Search></Search>
      <Definitions></Definitions>
    </div>
    
  </div>
</template>

<script>
import Search from './components/Search'
import Definitions from './components/Definitions'

export default {
  name: 'app',
  components: {
    Search, Definitions
  }
}
</script>

<style>

</style>
