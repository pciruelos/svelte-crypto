<script>
import { onMount } from "svelte";


  let titles = ["#", "Coin", "Price", "Change", "24h Volume"];
  let coins = [];
  let filterdCoins = [];
  let ref = null;

  const loadCoins = async () => {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    console.log(data);
    coins = data;
    filterdCoins = data;
  };
  loadCoins();

  
  const serachCoins = (value) => { 
    filterdCoins = coins.filter(c => c.name.toLowerCase().includes(value.toLowerCase()) || c.symbol.toLowerCase().includes(value.toLowerCase()))
  };

  onMount( () => {
    ref.focus()
  });

</script>

<main>
  <div class="container">
    <div class="row">
      <h1>Coin Market</h1>
      <input type="text" placeholder="Search your Coin" class="form-control bg-dark text-white rounded-0 border-0 my-4"
      on:keyup={({target:{value}})=> serachCoins(value)}
      bind:this={ref}
      >
      <table class="table table-dark">
        <thead>
          <tr>
            {#each titles as title}
              <th>{title}</th>
            {/each}
          </tr>
        </thead>
        <tbody>
          {#each filterdCoins as coin, i}
            <tr>
              <td class="text-muted">{i + 1}</td>
              <td><span><img src={coin.image} alt={coin.name}  style="width: 2rem;" class="img-fluid me-3"/>
                {coin.name}
              </span><span class="text-muted ms-2  text-uppercase">{coin.symbol}</span>
            </td>
            <td>${coin.current_price.toLocaleString()}</td>
            <td class={coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger'}>{coin.price_change_percentage_24h}%</td>
            <td>${coin.total_volume.toLocaleString()}</td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  </div>
</main>
