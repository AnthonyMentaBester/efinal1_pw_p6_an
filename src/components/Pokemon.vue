<template>
<div  class="container">
    <div class="puntaje">
        <h1  id="puntaje" >Puntaje:</h1>
        <label for="puntaje">0</label>

        <h1  id="intento">Intento:</h1>
        <label  for="intento">0</label>
    </div>

    <div class="adivina">

        <img id="img1" src="../assets/250.png" alt="no se pudo cargar">
        <label  for="img1" >xxxxxxxxx</label>
        <img  id="img2" src="../assets/250.png" alt="no se pudo cargar">
        <label for="img2">xxxxxxxxxxxx</label>
        <img id="img3" src="../assets/250.png" alt="no se pudo cargar">
        <label for="img3">xxxxxxxxxxxx</label>
    </div>

    <button class="boton" v-on="jugar" >JUGAR</button>

    <button class="resetear" v-on="nuevoJuego" >NUEVO JUEGO</button>
</div>  
</template>

<script>
export default {
    data() {
    return {
      nombre:"xxxxxxxxxxxx",
      img: null,
      puntos: 0,
      intentos: 0,
      msg: null,
    };
  },
   methods: {
    async jugar() {
      if (this.intentos === 0 || this.puntos >= 10) return;

      const ids = this.obtenerPokemonIds();
      const data = ids.map(id => fetch(`https://pokeapi.co/api/v2/pokemon/%7bidPokemon%7d`).then(res => res.json()));

      const pokemonsData = await Promise.all(data);

      this.pokemons = pokemonsData.map(pokemon => ({
        img: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/%7bidPokemon%7d.svg`,
        nombre: pokemon.nombre,
      }));

      this.calcularPuntaje();
      this.intentos -= 1;
    },
    obtenerPokemonIds() {
      const ids = [1, 2, 3, 4, 5];
      return Array(3).fill(null).map(() => ids[Math.floor(Math.random() * ids.length)]);
    },
    calcularPuntaje() {
      const nombres = this.pokemons.map(pokemon => pokemon.nombre);
    },
    nuevoJuego() {
      this.puntos = 0;
      this.intentos = 5;
      this.pokemons = Array(3).fill({ img: null, nombre: 'X' });
    },
  },
  computed:{
    ganaste(){
        if (this.intentos >= 5 || this.puntos >= 10){
            this.msg = "Felicitaciones has ganado un premio de $10.000.000"

        }

    },
    perdiste(){
        if (this.intentos === 5 || this.puntos >= 10){
            this.msg = "El juego ha terminado, intetaloo"
        }

    }

  }
}
</script>

<style>
.container{
    border: solid black;
}
.puntaje{
    display: flex;
    grid-template-columns: 2px;
    border-right: 20px;
    font-size: 10px;
    text-align: center;  
}
.adivina{
    display: flex;
    grid-template-columns: 20px 5px 5px;
    grid-template-rows: 2;
    margin: 10px 10px;
    padding: 10px 10px;
}

.boton{
    margin: 20px 20px;
    padding: 30px 50px;
    font-size: 30px;
    border: solid black;


}
.label{
    font-size: 20px;
    text-align: center;
}
.resetear{
    margin: 20px 20px;
    padding: 30px 50px;
    font-size: 30px;
    border: solid black;
    text-align: right;

}
.img{
    margin: 40px;
    align-content: center;
}
</style>