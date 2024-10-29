<script>
  import * as d3 from "d3";
  import { scaleLinear } from "d3";

  //importamos los JSONS con los datos
  import pelis from "/src/data/Set de datos.json";
  import gato1 from "/src/data/Gato con botas.json";
  import gato2 from "/src/data/Gato con botas 2.json";
  import shrek1 from "/src/data/Shrek 1.json";
  import shrek2 from "/src/data/Shrek 2.json";
  import shrek3 from "/src/data/Shrek 3.json";
  import shrek4 from "/src/data/Shrek 4.json";

  console.log(pelis);

  /* COLOR CATEGORICO */
  const colorEscena = d3
    .scaleOrdinal()
    .domain(["Crossover", "Ogro", "Otro", "Comico", "Musica"])
    .range(["#7CC2C1", "#8CB04E", "#F7F6E5", "#E38C6F", "#E1EE7D"]);

  /*TRANSFORMACION DE ESCALAS PELIS */
  let duracionGato2 = d3.sum(gato2, (d) => d.Duracion);
  const escGato2 = scaleLinear().domain([0, duracionGato2]).range([0, 100]);

  let duracionShrek1 = d3.sum(shrek1, (d) => d.Duracion);
  const escShrek1 = scaleLinear()
    .domain([0, duracionShrek1])
    .range([0, (100 * duracionShrek1) / duracionGato2]);

  let duracionShrek2 = d3.sum(shrek2, (d) => d.Duracion);
  const escShrek2 = scaleLinear()
    .domain([0, duracionShrek2])
    .range([0, (100 * duracionShrek2) / duracionGato2]);

  let duracionShrek3 = d3.sum(shrek3, (d) => d.Duracion);
  const escShrek3 = scaleLinear()
    .domain([0, duracionShrek3])
    .range([0, (100 * duracionShrek3) / duracionGato2]);

  let duracionShrek4 = d3.sum(shrek4, (d) => d.Duracion);
  const escShrek4 = scaleLinear()
    .domain([0, duracionShrek4])
    .range([0, (100 * duracionShrek4) / duracionGato2]);

  let duracionGato1 = d3.sum(gato1, (d) => d.Duracion);
  const escGato1 = scaleLinear()
    .domain([0, duracionGato1])
    .range([0, (100 * duracionGato1) / duracionGato2]);

  //variable orden para lelvar regstro de los estados (de esta manera puede cambiar lo de timeline y porcentaje). Su valor inicial es el que muestra los valores segun el timeline:
  let orden = "orden_inicio";

  //listas vacias para poder reordenar segun el inicio de las peliculas o las categorias (porcentaje)
  let sh1Orden = [];
  let sh2Orden = [];
  let sh3Orden = [];
  let sh4Orden = [];
  let gato1Orden = [];
  let gato2Orden = [];

  // lo que hicimos fue cambiar el $ --> por el const --> porque no lo reconocia, eso nos soluciono un error que saltaba siempre
  const sortData = () => {
    if (orden == "orden_inicio") {
      sh1Orden = d3.sort(shrek1, (escena) => escena.Inicio);
      sh2Orden = d3.sort(shrek2, (escena) => escena.Inicio);
      sh3Orden = d3.sort(shrek3, (escena) => escena.Inicio);
      sh4Orden = d3.sort(shrek4, (escena) => escena.Inicio);
      gato1Orden = d3.sort(gato1, (escena) => escena.Inicio);
      gato2Orden = d3.sort(gato2, (escena) => escena.Inicio);

      console.log(sh1Orden);
    } else if (orden == "orden_categoria") {
      sh1Orden = d3.sort(shrek1, (escena) => escena.Categoria);
      sh2Orden = d3.sort(shrek2, (escena) => escena.Categoria);
      sh3Orden = d3.sort(shrek3, (escena) => escena.Categoria);
      sh4Orden = d3.sort(shrek4, (escena) => escena.Categoria);
      gato1Orden = d3.sort(gato1, (escena) => escena.Categoria);
      gato2Orden = d3.sort(gato2, (escena) => escena.Categoria);

      console.log(sh1Orden);
    }
  };

  // Llamamos sortData para que lo llame con su valor inicial (el correspondinete a orden_inicio) sin tener que tocar ningun boton
  sortData();

  console.log("duracionShrek1", duracionShrek1);

  //REDONDEAMOS DIVIDIDO EN 10 LAS DURACIONES DE LAS PELICULAS PARA DAR UNA ESCALA DEBAJO DE LAS BARRAS
  let ticksSH1 = Math.floor(duracionShrek1 / 10) + 2;
  let ticksSH2 = Math.floor(duracionShrek2 / 10) + 1;
  let ticksSH3 = Math.floor(duracionShrek3 / 10) + 1;
  let ticksSH4 = Math.floor(duracionShrek4 / 10) + 1;
  let ticksG1 = Math.floor(duracionGato1 / 10) + 1;
  let ticksG2 = Math.floor(duracionGato2 / 10) + 1;
</script>

<!-- Prueba de header (al final decidimos hacerlo en el HTML)
<div id="container_links">
  <a href="#Shrek1">{pelis[0].Pelicula} </a>
  <a href="#Shrek2">{pelis[1].Pelicula} </a>
  <a href="#Shrek3">{pelis[2].Pelicula} </a>
  <a href="#Shrek4">{pelis[3].Pelicula} </a>
  <a href="#Gato">{pelis[4].Pelicula} </a>
  <a href="#Gato2">{pelis[5].Pelicula} </a>
</div> -->

<main>
  <div>
    <img
      src="./images/Frame 2.png"
      alt="Logo"
      style="width:80%; margin-top:3%; justify-content:center;"
    />
  </div>

  <!--BOTONES-->
  <div class="botones">
    <button
      class="MiBoton1"
      class:active={orden === "orden_inicio"}
      on:click={() => {
        orden = "orden_inicio";
        sortData(); // a diferencia de lo visto con carlos, aca llamamos a la CONST qeu se armo en el script para que se genere este "reordenamiento"
      }}
      >Cronología
    </button>

    <button
      class="MiBoton2"
      class:active={orden === "orden_categoria"}
      on:click={() => {
        orden = "orden_categoria";
        sortData();
      }}>Categorías</button
    >
  </div>

  <!-- SHREK 1 -->
  <h5 class="titulo" id="Shrek1">{pelis[0].Pelicula} ({pelis[0].Anio})</h5>
  <div class="container_movie">
    {#each sh1Orden as escena}
      <div
        class="escenaP"
        style="
            background-color:{colorEscena(escena.Categoria)}; 
            width: {escShrek1(escena.Duracion)}%; 
            height: 100px;
          "
      ></div>
    {/each}
    <!-- CHEQUEO DE CODIGO, si esta tomando bien el left: <div style="position: absolute; left: 100%; background-color: black; width: 30px; height: 100px;"></div> -->
  </div>

  <div id="container_ticks">
    {#each d3.range(ticksSH1) as t, i}
      <div class="ticks" style="width:{escShrek1(10)}%">{i * 10}</div>
    {/each}
  </div>

  <!-- SHREK 2 -->
  <h5 class="titulo" id="Shrek2">{pelis[1].Pelicula} ({pelis[1].Anio})</h5>
  <div class="container_movie">
    {#each sh2Orden as escena}
      <div
        class="escenaP"
        style="
          background-color:{colorEscena(escena.Categoria)}; 
          width: {escShrek2(escena.Duracion)}%; 
          height: 100px;
        "
      >
        <!-- chequeo <p>{escena.Categoria}</p> -->
      </div>
    {/each}
  </div>

  <div id="container_ticks">
    {#each d3.range(ticksSH2) as t, i}
      <div class="ticks" style="width:{escShrek2(10)}%">{i * 10}</div>
    {/each}
  </div>

  <!-- SHREK 3 -->
  <h5 class="titulo" id="Shrek3">{pelis[2].Pelicula} ({pelis[2].Anio})</h5>
  <div class="container_movie">
    {#each sh3Orden as escena}
      <div
        class="escenaP"
        style="
            background-color:{colorEscena(escena.Categoria)}; 
            width: {escShrek3(escena.Duracion)}%; 
            height: 100px;
          "
      ></div>
    {/each}
  </div>
  <div id="container_ticks">
    {#each d3.range(ticksSH3) as t, i}
      <div class="ticks" style="width:{escShrek3(10)}%">{i * 10}</div>
    {/each}
  </div>

  <!-- SHREK 4 -->
  <h5 class="titulo" id="Shrek4">{pelis[3].Pelicula} ({pelis[3].Anio})</h5>
  <div class="container_movie">
    {#each sh4Orden as escena}
      <div
        class="escenaP"
        style="
            background-color:{colorEscena(escena.Categoria)}; 
            width: {escShrek4(escena.Duracion)}%; 
            height: 100px;
          "
      ></div>
    {/each}
  </div>

  <div id="container_ticks">
    {#each d3.range(ticksSH4) as t, i}
      <div class="ticks" style="width:{escShrek4(10)}%">{i * 10}</div>
    {/each}
  </div>

  <!-- GATO 1 -->
  <h5 class="titulo" id="Gato">{pelis[4].Pelicula} ({pelis[4].Anio})</h5>
  <div class="container_movie">
    {#each gato1Orden as escena}
      <div
        class="escenaP"
        style="
            background-color:{colorEscena(escena.Categoria)}; 
            width: {escGato1(escena.Duracion)}%; 
            height: 100px;
          "
      ></div>
    {/each}
  </div>

  <div id="container_ticks">
    {#each d3.range(ticksG1) as t, i}
      <div class="ticks" style="width:{escGato1(10)}%">{i * 10}</div>
    {/each}
  </div>

  <!-- GATO 2 -->
  <h5 class="titulo" id="Gato2">
    {pelis[5].Pelicula} ({pelis[5].Anio})
  </h5>
  <div class="container_movie">
    {#each gato2Orden as escena}
      <div
        class="escenaP"
        style="
            background-color:{colorEscena(escena.Categoria)}; 
            width: {escGato2(escena.Duracion)}%; 
            height: 100px;
          "
      ></div>
    {/each}
  </div>

  <div id="container_ticks">
    {#each d3.range(ticksG2) as t, i}
      <div class="ticks" style="width:{escGato2(10)}%">{i * 10}</div>
    {/each}
  </div>
</main>

<style>
  .container_movie {
    /*position: relative;*/
    height: 100px;
    margin-bottom: 20px;
    display: flex;
    flex-direction: row;
  }
  /* .escena {
    position: absolute;
    height: 100%;
    transition: all 0.5s;
  }
    */

  /*En este caso ahora la clase se llama escenaP porque empezo siendo una prueba cuando veiamos que no nos andaba inicialmente el codigo*/

  .escenaP {
    height: 100%;
    transition: all 0.5s;
    left: float;
    position: relative;
  }

  #container_ticks {
    height: 20px;
    margin-bottom: 20px;
    flex-direction: row;
  }

  .ticks {
    position: relative;
    border-left: black;
    height: 20px;
    float: left;
    font-size: 10px;
  }
  .ticks::after {
    content: "";
    position: absolute;
    left: 0;
    top: -10px;
    height: 10px;
    border-left: 1px solid black;
    margin-bottom: 0.7px;
  }

  .ticks:last-child{
    width: 10px !important;
    word-wrap: normal;
  }

  main {
    max-width: 1280px;
    margin: 0 auto;
    margin-left: 13%;
    margin-right: 10%;
  }
  /* #container_links {
    width: 100%;
    align-items: center;
  } */
  .botones {
    display: flex;
    gap: 8px;
  }

  .MiBoton1:hover {
    background-color: rgb(164, 173, 44);
    color: #f7f6e5;
  }

  .MiBoton2:hover {
    background-color: rgb(164, 173, 44);
    color: #f7f6e5;
  }

  .MiBoton1 {
    margin-top: 2%;
    width: 10%;
    height: 30px;
    background-color: #f7f6e5;
    color: #bbc61a;
    display: flex;
    justify-content: center; /* Centrado horizontal */
    align-items: center; /* Centrado vertical */
    border-radius: 5px;
    border: 1.4px, solid, rgb(120, 127, 32);
    font-size: 100%;
  }
  .MiBoton2 {
    margin-top: 2%;
    width: 10%;
    height: 30px;
    background-color: #f7f6e5;
    color: #bbc61a;
    display: flex;
    justify-content: center; /* Centrado horizontal */
    align-items: center; /* Centrado vertical */
    border-radius: 5px;
    border: 1.4px, solid, rgb(120, 127, 32);
  }
  .active {
    margin-top: 2%;
    width: 10%;
    height: 30px;
    background-color: #bbc61a;
    color: #f7f6e5;
    display: flex;
    justify-content: center; /* Centrado horizontal */
    align-items: center; /* Centrado vertical */
    border-radius: 5px;
  }

  img {
    margin-left: 10%;
    margin-right: 10%;
    margin-bottom: 30px;
  }

  /* a {
    margin-right: 4%;
    text-decoration: none;
    text-align: center;
    justify-content: center;
  }
  a:hover {
    text-decoration: underline;
  } */

  /* .MiBoton {
    margin-top: 2%;
    width: 10%;
    height: 30px;
    background-color: #e1ee7d;
    color: #686e1b;
  } */
</style>
