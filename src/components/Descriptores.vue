<template>
  <div class="container">
    <h1 class="is-size-3">Descriptores</h1>
    <b-field grouped>
      <b-field label="Base / Inicio" expanded>
        <input
          class="input"
          @change="calulate"
          placeholder="base"
          v-model="base"
          type="text"
        />
      </b-field>
      <b-field label="Final" expanded>
        <input
          class="input"
          @change="calulate"
          placeholder="final"
          v-model="final"
          type="text"
        />
      </b-field>
      <b-field label="Limite" expanded>
        <input
          disabled
          class="input"
          placeholder="limit"
          v-model="limit"
          type="text"
        />
      </b-field>
    </b-field>

    <div class="table-container">
      <table align="center" class="table is-bordered is-fullwidth">
        <tbody>
          <tr>
            <td>Base (Posición)</td>
            <td v-for="(b, i) in base2Bin" :key="i">
              <small> {{ base2Bin.length - i - 1 }} </small>
            </td>
          </tr>
          <tr>
            <td>Base (Binario)</td>
            <td v-for="(bit, i) in base2Bin" :key="i">
              <small> {{ bit }} </small>
            </td>
          </tr>
        </tbody>
      </table>
      <table align="center" class="table is-bordered">
        <tbody>
          <tr>
            <td>Límite (Posición)</td>
            <td v-for="(b, i) in limit2Bin" :key="i">
              <small>{{ limit2Bin.length - i - 1 }} </small>
            </td>
          </tr>
          <tr>
            <td>Límite (Binario)</td>
            <td v-for="(bit, i) in limit2Bin" :key="i">
              <small> {{ bit }} </small>
            </td>
          </tr>
        </tbody>
      </table>

      <h2 class="is-size-4">8086 - 80286 / 80386 - Pentium 4</h2>
      <img
        src="https://user-images.githubusercontent.com/36168284/81505227-6173c600-92b3-11ea-972c-c7790674c044.png"
        alt=""
      />
      <img
        src="https://user-images.githubusercontent.com/36168284/81505240-72bcd280-92b3-11ea-9c5f-ce56f873c24a.png"
        alt=""
      />

      <b-field grouped>
        <b-field expanded>
          <b-checkbox v-model="AV" size="is-small"
            >Segmento disponible? (AV)</b-checkbox
          >
        </b-field>
        <b-field label="Modo de acceso (D):">
          <b-radio size="is-small" v-model="D" :native-value="false"
            >16 bits</b-radio
          >
          <b-radio size="is-small" v-model="D" :native-value="true"
            >32 bits</b-radio
          >
        </b-field>

        <b-field expanded>
          <b-checkbox v-model="G" size="is-small"
            >Limite > 20 bits (G=1)</b-checkbox
          >
        </b-field>
      </b-field>

      <h2 class="is-size-4">Derechos de acceso</h2>
      <b-field grouped>
        <b-field expanded>
          <b-checkbox v-model="P" size="is-small"
            >Base y limite válidos (P)</b-checkbox
          >
        </b-field>

        <b-field>
          <b-checkbox v-model="S" size="is-small"
            >Descriptor de sistema (S)</b-checkbox
          > </b-field
        ><b-field expanded>
          <b-checkbox v-model="E" size="is-small"
            >Segmento de código (E)</b-checkbox
          >
        </b-field>
        <b-field expanded>
          <b-checkbox v-model="ED" size="is-small"
            >Se expande hacia abajo (ED)</b-checkbox
          >
        </b-field>
      </b-field>

      <b-field grouped>
        <b-field expanded>
          <b-checkbox v-model="C" size="is-small"
            >Nivel de privilegio (C)</b-checkbox
          >
        </b-field>
        <b-field expanded>
          <b-checkbox v-model="W" size="is-small"
            >Puede escribirse (W)</b-checkbox
          >
        </b-field>

        <b-field expanded>
          <b-checkbox v-model="R" size="is-small">Puede leerse (R)</b-checkbox>
        </b-field>

        <b-field expanded>
          <b-checkbox v-model="A" size="is-small"
            >Ah sido accesido (A)</b-checkbox
          >
        </b-field>
      </b-field>

      <table align="center" class="table is-bordered">
        <tbody>
          <tr>
            <td v-for="(td, i) in 16" :key="i">
              <small>{{ 16 - i - 1 }}</small>
            </td>
          </tr>
          <tr>
            <td colspan="8">
              <!-- <input class="input" placeholder="Base (B31-B24)" type="text" /> -->
              <span
                v-for="(pos, i) in [32, 31, 30, 29, 28, 27, 26, 25]"
                :key="i"
              >
                {{ base2Bin[base2Bin.length - pos] || 0 }}
              </span>
            </td>
            <td colspan="1">{{ G ? 1 : 0 }}</td>
            <td colspan="1">{{ D ? 1 : 0 }}</td>
            <td colspan="1">0</td>
            <td colspan="1">{{ AV ? 1 : 0 }}</td>
            <td colspan="4">
              <!-- <input class="input" placeholder="Limite (L19-L16)" type="text" /> -->
              <span v-for="(pos, i) in [20, 19, 18, 17]" :key="i">
                {{ limit2Bin[limit2Bin.length - pos] || 0 }}
              </span>
            </td>
          </tr>

          <tr>
            <td colspan="1">{{ P ? 1 : 0 }}</td>
            <td colspan="2">{{ DPL }}</td>
            <td colspan="1">{{ S ? 0 : 1 }}</td>
            <td colspan="1">{{ E ? 1 : 0 }}</td>
            <td colspan="1">{{ ED || C ? 1 : 0 }}</td>
            <td colspan="1">{{ R || W ? 1 : 0 }}</td>
            <td colspan="1">{{ A ? 1 : 0 }}</td>

            <td colspan="8">
              <!-- <input class="input" placeholder="Limite (L15-L0)" type="text" :value="limit2Bin" /> -->
              <span
                v-for="(pos, i) in [24, 23, 22, 21, 20, 19, 18, 17]"
                :key="i"
              >
                {{ base2Bin[base2Bin.length - pos] || 0 }}
              </span>
            </td>
          </tr>
          <tr>
            <td colspan="16">
              <!-- <input class="input" placeholder="Limite (L15-L0)" type="text" :value="limit2Bin" /> -->
              <span
                v-for="(pos, i) in [
                  16,
                  15,
                  14,
                  13,
                  12,
                  11,
                  10,
                  9,
                  8,
                  7,
                  6,
                  5,
                  4,
                  3,
                  2,
                  1
                ]"
                :key="i"
              >
                {{ base2Bin[base2Bin.length - pos] || 0 }}
              </span>
            </td>
          </tr>
          <tr>
            <td colspan="16">
              <!-- <input class="input" placeholder="Limite (L15-L0)" type="text" :value="limit2Bin" /> -->
              <span
                v-for="(pos, i) in [
                  16,
                  15,
                  14,
                  13,
                  12,
                  11,
                  10,
                  9,
                  8,
                  7,
                  6,
                  5,
                  4,
                  3,
                  2,
                  1
                ]"
                :key="i"
              >
                {{ limit2Bin[limit2Bin.length - pos] || 0 }}
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <img
      src="https://user-images.githubusercontent.com/36168284/81522986-15a43980-9312-11ea-9ebe-13bcf2cbfeec.png"
      alt=""
    />
  </div>
</template>

<script>
export default {
  name: 'Direc',
  data () {
    return {
      segment: '',
      G: false,
      D: false,
      AV: false,
      A: false,
      E: false,
      ED: false,
      W: false,
      C: false,
      R: false,
      S: false,
      DPL: '00',
      P: true,
      base: '03000000',
      limit: '',
      final: '05FFFFFF'
    }
  },
  created () {
    this.calulate()
  },
  watch: {},
  methods: {
    calulate () {
      this.limit = parseInt(this.final, 16) - parseInt(this.base, 16)
      this.limit = this.limit.toString(16).toUpperCase()
      if (this.limit.length > 5) {
        console.log(this.limit)
        this.G = true
        this.limit = this.limit.slice(0, -3)
      } else {
        this.G = false
      }
    },
    hex2bin (hex) {
      return parseInt(hex, 16)
        .toString(2)
        .padStart(8, '0')
    }
  },
  computed: {
    base2Bin () {
      return this.hex2bin(this.base)
    },
    limit2Bin () {
      return this.hex2bin(this.limit)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
table td:not([align]),
table th:not([align]) {
  text-align: center;
}
</style>
