<script>
  import Header from "$lib/Header.svelte";

  // go to the getWalls function and it will send you back the array.
  let carsForHire = getCars();
  // let selected = carsForHire[0];

  async function getCars() {
    // hop off to the website to get the array
    let shopData = await fetch(
      "https://digitech.craighead.school.nz/api/car-hire",
    );

    // send the array to where this function was called from.
    return shopData.json();
  }
  let cart = [];
  function addCarToCart(carsForHire) {
    cart = [...cart, carsForHire];
    calcTotalCost();
  }

  let totalCost = 0;
  function calcTotalCost() {
    totalCost = 0;
    for (let carsForHire of cart) {
      totalCost = totalCost + carsForHire.price;
    }
  }
</script>

<Header />
<div class="columns">
  <div class="column">
    <!-- While you're still waiting for the data from the other website, show a waiting message -->
    {#await carsForHire}
      ...waiting

      <!-- once you get the data, do this stuff -->
    {:then carsForHire}
      {#each carsForHire.fast as carsForHire}
        {#if carsForHire.available}
          {carsForHire.car}
          {carsForHire.description}
          {carsForHire.price}
          <img src={carsForHire.img} alt={carsForHire.car} />
          {carsForHire.credit}
          {#if cart.length < 3}
            <button
              on:click={() => {
                addCarToCart(carsForHire);
              }}>+</button
            >
          {/if}
        {/if}
      {/each}
      {#each carsForHire.nice as carsForHire}
        {#if carsForHire.available}
          {carsForHire.car}
          {carsForHire.description}
          {carsForHire.price}
          <img src={carsForHire.img} alt={carsForHire.car} />
          {carsForHire.credit}
          {#if cart.length < 3}
            <button
              on:click={() => {
                addCarToCart(carsForHire);
              }}>+</button
            >
            <!-- {#elseif cart.length == 3}
              <p>Cart is Full</p>
            {/if} -->
            <!-- :else {cart.length == 2}
            <p>Cart is Full</p> -->
            <!-- {cart.length} -->
          {/if}
        {/if}
      {/each}
    {/await}
  </div>
  <div class="column">
    <h2>Total Cost of Cart =$ {totalCost}</h2>
    <p>There is {cart.length} items in Cart</p>
    {#if cart.length == 0}
      <p>Cart is Empty</p>
    {/if}
    <!-- {#elseif cart.length > 0 && < 3} -->

    {#if cart.length == 3}
      <p>Cart is Full</p>
      <p>Checkout cars or remove some from cart</p>
    {/if}
    {#each cart as carsForHire}
      {carsForHire.car}
      {carsForHire.description}
      {carsForHire.price}
      <img src={carsForHire.img} alt={carsForHire.car} />
      {carsForHire.credit}
      {carsForHire.available}
    {/each}
  </div>
</div>

<footer>
  <p>&copy; AndraComan @ Craighead Diocesan School 2024</p>
</footer>

<style>
  .column {
    width: 400px; /* Fixed width of 1000px */
    max-width: 100%; /* Ensures the container doesn't overflow */
    margin: 10px auto; /* Centers the container */
  }
</style>
