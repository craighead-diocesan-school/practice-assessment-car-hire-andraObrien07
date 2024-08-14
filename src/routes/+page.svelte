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

  //opens a cart array
  let cart = [];
  //calling car and adding it to cart
  function addCarToCart(carsForHire) {
    cart = [...cart, carsForHire];
    //triggering the calculate total cost of cart
    calcTotalCost();
  }

  //getting rid of magic numbers
  let maxNumbOfCarsInCart = 3;

  //getting rid of majic numbers
  let totalCost = 0;

  // calculating the total cost of cart
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
      <!-- two diff loops to go through the two diff arrays. fast and nice -->
      {#each carsForHire.fast as carsForHire}
        <!-- if the car is available, display its information -->
        {#if carsForHire.available}
          {carsForHire.car}
          {carsForHire.description}
          {carsForHire.price}
          <img src={carsForHire.img} alt={carsForHire.car} />
          {carsForHire.credit}
          <!-- if the car length is equal to maxNumbOfCarsInCart disable button, if not have the button to be clicked -->
          <button
            disabled={cart.length == maxNumbOfCarsInCart}
            on:click={() => {
              addCarToCart(carsForHire);
            }}>+</button
          >
        {/if}
      {/each}
      {#each carsForHire.nice as carsForHire}
        {#if carsForHire.available}
          {carsForHire.car}
          {carsForHire.description}
          {carsForHire.price}
          <img src={carsForHire.img} alt={carsForHire.car} />
          {carsForHire.credit}

          <button
            disabled={cart.length == maxNumbOfCarsInCart}
            on:click={() => {
              addCarToCart(carsForHire);
            }}>+</button
          >
        {/if}
      {/each}
    {/await}
  </div>
  <div class="column">
    <h2>Total Cost of Cart =$ {totalCost}</h2>
    {#if cart.length == 0}
      <p>Cart is Empty</p>
    {:else if cart.length == maxNumbOfCarsInCart}
      <p>Cart is Full</p>
    {:else}
      <p>There is {cart.length} items in Cart</p>
    {/if}

    <!-- {#if cart.length == maxNumbOfCarsInCart}
      <p>Cart is Full</p>
      <p>Checkout cars or remove some from cart</p>
    {/if} -->
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
