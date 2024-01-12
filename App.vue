<template>
  <div class="page">

    <div v-if="atrrAtivo1 || atrrAtivo2">{{ativo}}</div>

    <div @click="cleanFilter()">Limpar filtro</div>

     <!-- marcas -->

    <div class="itens" v-for="(marca, i) in produtos" :key="i">
      <!-- <a :href="`http://localhost:3000/produtos?marca=${marca.marca}`">{{ marca.marca }}</a> -->
      <!-- <p @click="fetchFiltro(marca.marca)">{{ marca.marca }}</p> -->
      <p class="itens-marca" v-if="!atrrAtivo1" 
      @click="fetchFiltro(this.url+'&marca='+marca.marca), checkActive(marca.marca, 1)">
        {{ marca.marca }}
      </p>
      <!-- {{this.url+'&marca='+marca.marca}} -->
    </div>

    {{this.url}}

    <!-- <div class="itens" v-for="(marca, i) in produtos" :key="i">
      <input type="checkbox" :id="'marcas'+i" :value="'?marca='+marca.marca" v-model="selected.marcas">
      <label :for="'marcas'+i">{{ marca.marca }}</label>
      {{ marca.value }}
    </div> -->

<!-- https://codepen.io/itrong/pen/xLvowZ -->
    <!-- selected.marcas {{selected.marcas}} -->

    <!-- cor  -->

    <!-- <div class="caixa" v-for="(cor, i) in produtos" :key="i">
      <p>{{cor.cor}}</p>
    </div> -->

    <!-- https://pt.stackoverflow.com/questions/16483/remover-elementos-repetido-dentro-de-um-array-em-javascript -->

    <div class="cor" v-for="(cor, i) in atributoCor" :key="i">
      <p v-if="!atrrAtivo2" @click="fetchFiltro(this.url+'&cor='+cor), checkActive(cor, 2)">{{cor}}</p>
    </div>

    <!-- cores: {{this.cores}} -->

    <div class="caixa" v-for="(prod, i) in prodFilter" :key="i">
      <p>{{ prod.nome }}</p>
      <p>{{ prod.preco }}</p>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      produtos: [],
      prodFilter: [],
      url: 'http://localhost:3000/produtos?estoque=true',
      ativo: [],
      cores: [],
      atrrAtivo1: false,
      atrrAtivo2: false,
      selected: {
        marcas: [

        ]
      }
    }
  },
  created(){
    
  },
  computed: {
    atributoCor(){
      return this.cores.filter((item, i) => this.cores.indexOf(item) === i)
      // cores: [ "preto", "branco", "preto", "preto", "branco", "branco" ]
    }
  },
  methods: {
    fetchFiltro(marca){
      fetch(marca)
        .then(r => r.json())
        .then(r => {
          this.prodFilter = r;
          this.url = marca;
        }).catch(err => console.log(err.message))
    },
    checkActive(attr, number){
      this.ativo.push(attr);
      if(number == 1){
        this.atrrAtivo1 = true;
      } else {
        this.atrrAtivo2 = true;
      }
    },
    cleanFilter(){
      this.url = 'http://localhost:3000/produtos?estoque=true'
      this.atrrAtivo1 = false;
      this.atrrAtivo2 = false;
    }
  },
  mounted(){
      fetch("http://localhost:3000/produtos?estoque=true")
        .then(r => r.json())
        .then(r => {
          this.produtos = r;
          this.produtos.forEach(item => {
            this.cores.push(item.cor);
          });
        }).catch(err => console.log(err.message))
  }
}
</script>

<style>
.caixa{
  margin: 100px;
  display: inline-block;
  cursor: pointer;
}
.itens{
  cursor: pointer;
}
.itens p {
  margin: 10px;
}
.cor{
  margin: 50px;
}
</style>
