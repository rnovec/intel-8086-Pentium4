<template>
  <div class="container">
    <h1 class="is-size-3">Direccionamiento (Modo Real y Protegido)</h1>
    <b-field grouped>
      <b-field label="Segmento" expanded>
        <input
          @change="calculate"
          class="input"
          placeholder="Modo real"
          v-model="segment"
          type="text"
        />
      </b-field>
      <b-field label="Base / Inicio" expanded>
        <input
          @change="calculate"
          class="input"
          placeholder="Modo protegido"
          v-model="base"
          type="text"
        />
      </b-field>
      <b-field label="Limite" expanded>
        <input
          @change="calculate"
          class="input"
          placeholder="limite"
          v-model="limit"
          type="text"
        />
      </b-field>
      <b-field label="Final" expanded>
        <input
          disabled
          @change="calculate"
          class="input"
          placeholder="final"
          v-model="final"
          type="text"
        />
      </b-field>
      <b-field label="Gran." expanded>
        <input
          @change="calculate"
          class="input"
          placeholder="gran."
          v-model="gran"
          min="0"
          max="1"
          type="number"
        />
      </b-field>
    </b-field>
    <em>Final = Base + Limite = {{ result }}</em> 
  </div>
</template>

<script>
export default {
  name: 'Direc',
  data () {
    return {
      segment: '',
      gran: 0,
      base: 'A00000',
      limit: '1000',
      final: 0,
      res: 0
    }
  },
  created () {
    this.calculate()
  },
  methods: {
    dec2Hex(dec) {
      return dec.toString(16).toUpperCase()
    },
    calculate () {
      const base = parseInt(this.base, 16) 
      if (this.gran == '1' && !this.segment) { // protected mode
        const limitWithG = parseInt(this.limit + 'FFF', 16) // add FFFH
        console.log('G=' + this.dec2Hex(limitWithG))
        this.final = base + limitWithG
        this.segment = ''
      } else if (!this.segment) {
        this.segment = ''
        const limit = parseInt(this.limit, 16)
        this.final = base + limit

      } else { // real mode
        this.limit = ''
        this.base = this.segment + '0' // add 0H
        this.final = base + parseInt('FFFF', 16) // add FFFF
        this.base = this.dec2Hex(this.base) // base to hex
      }
      this.final = this.dec2Hex(this.final) // final to hex
    }
  },
  computed: {
    result () {
      return `${this.final}H`
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
