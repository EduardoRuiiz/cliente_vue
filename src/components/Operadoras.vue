<template>
  <div>
    <h1>{{ msg }}</h1>
    <label>
      <p>
        Digite um
        Nome Fantasia,
        Razao Social,
        Regiao de Comercializacao,
        Modalidade,
        Cidade,
        UF ou
        Bairro
        para realizar a busca.
        <br />
        Ou deixe em branco para listar todas as operadoras.
      </p>
      <input type="text" v-model="termoBusca" />
    </label>
    <button @click="buscar"> Buscar </button>
    <table v-if="resultados.length">
      <thead>
        <tr>
          <th v-for="(coluna, index) in colunas" :key="index">{{ coluna }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(registro, i) in resultados" :key="i">
          <td v-for="(coluna, j) in colunas" :key="j">
            {{ registro[coluna] }}
          </td>
        </tr>
      </tbody>
    </table>
    <p v-if="erro" style="color: red;">{{ erro }}</p>
  </div>
</template>

<script>
export default {
  name: 'FiltroOperadoras',
  props: {
    msg: String
  },
  data() {
    return {
      termoBusca: '',
      resultados: [],
      colunas: [],
      erro: ''
    }
  },
  methods: {
    async buscar() {
      this.resultados = []
      this.colunas = []
      this.erro = ''

      try {
        const res = await fetch(`http://localhost:5000/operadoras?busca=${encodeURIComponent(this.termoBusca)}`)
        if (!res.ok) throw new Error('Erro na requisição')

        const json = await res.json()
        this.resultados = json
        if (json.length > 0) {
          this.colunas = Object.keys(json[0])
        }
        console.log(this.resultados);

      } catch (e) {
        this.erro = 'Erro ao buscar operadoras. verifique se o servidor está rodando.'
        console.error(e)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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

input {
  width: 510px;
}

table {
  margin-top: 24px;
  border-collapse: collapse;
  width: 100%;
}

th,
td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: left;
}

th {
  background: #f5f5f5;
}
</style>
