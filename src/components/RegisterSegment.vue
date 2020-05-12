<template>
  <div class="container">
    <h1 class="is-size-4">Tablas, selectores y niveles de privilegio</h1>
    <b-field grouped>
      <b-field label="DS">
        <input
          @change="calculate"
          class="input"
          placeholder="Modo protegido"
          v-model="ds"
          type="text"
        />
      </b-field>
    </b-field>
    <table align="center" class="table is-bordered">
      <tbody>
        <tr></tr>
        <tr>
          <th colspan="13">Selector</th>
          <th colspan="1">TI</th>
          <th colspan="2">RPL</th>
        </tr>
        <tr>
          <td colspan="13">{{ selector }}</td>
          <td colspan="1">{{ TI }}</td>
          <td colspan="2">{{ RPL }}</td>
        </tr>
        <tr>
          <td colspan="13">{{ bin2dec(selector) }}</td>
          <td colspan="1">{{ bin2dec(TI) }}</td>
          <td colspan="2">{{ bin2dec(RPL) }}</td>
        </tr>
      </tbody>
    </table>
    <img
      src="https://user-images.githubusercontent.com/36168284/81523036-3e2c3380-9312-11ea-9dcf-6032d69c6a85.png"
      alt=""
    />
  </div>
</template>

<script>
export default {
  name: 'Direc',
  data () {
    return {
      ds: '0105'
    }
  },
  methods: {
    bin2dec (bin) {
      return parseInt(bin, 2)
    },
    hex2bin (hex) {
      return parseInt(hex, 16)
        .toString(2)
        .padStart(8, '0')
    },
    calculate () {}
  },
  computed: {
    selector () {
      return this.hex2bin(this.ds).slice(0, -3)
    },
    TI () {
      return this.hex2bin(this.ds).slice(-3, -2)
    },
    RPL () {
      return this.hex2bin(this.ds).slice(-2)
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
