<template>
  <div id="app">
    <div class="is-offset-one-quarter ">
      <img class="column is-half is-offset-one-quarter" src="./assets/pokemon-logo.jpg" id="pokemonimg" />
      <hr />
      <input
        type="text"
        class="input is-rounded column is-half is-offset-one-quarter"
        placeholder="Buscar Pokemón Pelo Nome"
        v-model="busca"
        id="imput"
      />
      <hr/>

      <div id="pokemondiv" >
        <div v-for="(poke, index) in resultadoBusca" :key="poke.url" id="pokemon" class="column is-half is-one-third">
          <pokemon :name="poke.name" :url="poke.url" :num="index + 1"/>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import Pokemon from "./components/Pokemon.vue";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      busca: "",
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log("Pegou a lista de pokemons");
        this.pokemons = res.data.results;
        console.log(this.pokemons);
      });
  },
  components: {
    Pokemon,
  },
  computed: {
    resultadoBusca: function(){
      if(this.busca == ""|| this.busca == " "){
        return this.pokemons
      }else{
        return this.pokemons.filter((pokemon) =>
          pokemon.name.toLowerCase().includes(this.busca.toLowerCase())
        );
      }
    }
  }
};
</script>

<style>
#app {
  background-color: rgba(0, 0, 0, 0.555);
 
}
#pokemonimg {
  border-radius: 3px;
}
#imput {
  text-align: center;
}

#pokemon{
  text-align: center;
margin: 0px;
}
#pokemondiv{
display: flex;
flex-wrap: wrap;
align-self: center;
}

</style>
