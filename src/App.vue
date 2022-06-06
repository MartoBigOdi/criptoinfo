<template>
<div class="container">
<div class="container p-3 my-4 text-muted">
    <h1> VALORES DE LAS CRIPTO  <svg xmlns="http://www.w3.org/2000/svg" width="50" height="50" fill="currentColor" class="bi bi-currency-bitcoin" viewBox="0 0 16 16">
  <path d="M5.5 13v1.25c0 .138.112.25.25.25h1a.25.25 0 0 0 .25-.25V13h.5v1.25c0 .138.112.25.25.25h1a.25.25 0 0 0 .25-.25V13h.084c1.992 0 3.416-1.033 3.416-2.82 0-1.502-1.007-2.323-2.186-2.44v-.088c.97-.242 1.683-.974 1.683-2.19C11.997 3.93 10.847 3 9.092 3H9V1.75a.25.25 0 0 0-.25-.25h-1a.25.25 0 0 0-.25.25V3h-.573V1.75a.25.25 0 0 0-.25-.25H5.75a.25.25 0 0 0-.25.25V3l-1.998.011a.25.25 0 0 0-.25.25v.989c0 .137.11.25.248.25l.755-.005a.75.75 0 0 1 .745.75v5.505a.75.75 0 0 1-.75.75l-.748.011a.25.25 0 0 0-.25.25v1c0 .138.112.25.25.25L5.5 13zm1.427-8.513h1.719c.906 0 1.438.498 1.438 1.312 0 .871-.575 1.362-1.877 1.362h-1.28V4.487zm0 4.051h1.84c1.137 0 1.756.58 1.756 1.524 0 .953-.626 1.45-2.158 1.45H6.927V8.539z"/>
</svg> </h1> 
    <h4>By <a href="https://www.coingecko.com/en">CoinsGecko</a></h4>
</div>
    
    <div class="row">

      <input type="text" class="form-control bg-dark text-light rounded-0 border-0 my-2" 
      placeholder="Buscar Moneda"
       @keyup="buscarMoneda()"
       v-model="monedaBuscada"
      >

        <table class="table table-dark">
            <thead>
                <tr>
                    <th v-for="title in titles" :key="title">
                        {{title}}
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(coin, index) in filtradasMonedas" :key="coin.id">
                    <td class="text-muted">
                      {{index + 1}}
                    </td>
                    <td>
                      <img :src="coin.image" style="width:2rem">
                       <span>
                    {{ coin.name }}
                       </span>
                       <span class ="ms-2 text-uppercaes text-muted">
                       {{coin.symbol}}
                       </span>
                    </td>
                    <td>
                     $ {{coin.current_price}}
                    </td>
                    <td :class="[coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger']">
                    {{coin.price_change_percentage_24h}} %
                    </td>
                     <td>
                     $ {{coin.total_volume.toLocaleString()}}
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</div>


</template>

<script>
export default {
    name: "App",
    data() {
        return {
            coins: [],
            filtradasMonedas: [],
            titles: [ "#", "Moneda", "Precio", "Precio Cambio", "24hs Volumen" ],
            monedaBuscada:'',
        };
    },
    async mounted() {
        //Pedimos los datos, los formateamos en formato json y los mostranmos por consola.
        const res = await fetch(
            "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
        );
        const data = await res.json();
        console.log(data);
        this.coins = data;
        this.filtradasMonedas = data;
    },
    methods:{
         buscarMoneda(){ //Acá lo que hacemos es filtrar los datos por minisculas o por menos letras que el total de letras de la palabra
            this.filtradasMonedas = this.coins.filter((coin) => 

            coin.name.toLowerCase().includes(this.monedaBuscada.toLowerCase()) || coin.symbol.toLowerCase().includes(this.monedaBuscada.toLowerCase()));
      },
   },
};
</script>

<style>
</style>
