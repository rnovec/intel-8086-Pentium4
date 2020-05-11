<template>
  <div class="container">
    <h1 class="is-size-3">Direccionamiento</h1>
    <b-field grouped>
      <b-field label="Segmento" expanded>
        <input
          @change="calculate"
          class="input"
          placeholder="segmento"
          v-model="segment"
          type="text"
        />
      </b-field>
      <b-field label="Base / Inicio" expanded>
        <input
          @change="calculate"
          class="input"
          placeholder="base"
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
    {{ result }}
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
    calculate () {
      if (this.gran == '1' && !this.segment) {
        this.segment = ''
        this.final = parseInt(this.base, 16) + parseInt(this.limit + 'FFF', 16)
      } else if (!this.segment) {
        this.segment = ''
        this.final = parseInt(this.base, 16) + parseInt(this.limit, 16)
      } else {
        this.limit = ''
        this.base = this.segment + '0'
        this.final = parseInt(this.base, 16) + parseInt('FFFF', 16)
        this.base = this.base.toString(16).toUpperCase()
      }
      this.final = this.final.toString(16).toUpperCase()
    }
  },
  computed: {
    result () {
      return `${this.base}H - ${this.final}H`
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
