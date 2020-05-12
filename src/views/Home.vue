<template>
  <div>
    <Addressing/>
    <hr>
    <ProtectedMode/>
    <hr>
    <RegisterSegment/>
    <hr>
  </div>
</template>

<script>
// @ is an alias to /src
import Addressing from '@/components/Addressing.vue'
import ProtectedMode from '@/components/ProtectedMode.vue'
import RegisterSegment from '@/components/RegisterSegment.vue'

export default {
  name: 'Home',
  components: {
    Addressing,
    ProtectedMode,
    RegisterSegment
  }
}
</script>
