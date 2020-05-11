<template>
  <div>
    <Direc/>
    <hr>
    <Descriptores/>
    <hr>
  </div>
</template>

<script>
// @ is an alias to /src
import Direc from '@/components/Direc.vue'
import Descriptores from '@/components/Descriptores.vue'

export default {
  name: 'Home',
  components: {
    Direc,
    Descriptores
  }
}
</script>
