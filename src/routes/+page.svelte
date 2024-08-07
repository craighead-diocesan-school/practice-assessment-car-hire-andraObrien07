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
</script>

<Header />

<!-- {selected}  -->
<!-- <select bind:value={selected}>
  {#each carsForHire as cars}
    <option value={cars}> {cars.name} </option>
  {/each}
</select> -->

<!-- While you're still waiting for the data from the other website, show a waiting message -->
{#await carsForHire}
  ...waiting

  <!-- once you get the data, do this stuff -->
{:then carsForHire}
  {#each carsForHire.fast as carsForHire}
    <!-- create a card for each item in the array (we called it 'wall') and send the info to the card component -->

    {carsForHire.car}
    {carsForHire.description}
    {carsForHire.price}
    <img src={carsForHire.img} alt={carsForHire.car} />
    {carsForHire.credit}
    {carsForHire.available}
  {/each}
  {#each carsForHire.nice as carsForHire}
    <!-- create a card for each item in the array (we called it 'wall') and send the info to the card component -->

    {carsForHire.car}
    {carsForHire.description}
    {carsForHire.price}
    <img src={carsForHire.img} alt={carsForHire.car} />
    {carsForHire.credit}
    {carsForHire.available}
  {/each}
{/await}

<!-- this code below works!!!!!!! -->
<!-- <select bind:value={selected}>
  <option value="nice">nice</option>
  <option value="fast">fast</option>
</select> -->

<!-- {#each selected.cars as item, index}
 
  <input bind:value={item} />
{/each} -->

<footer>
  <p>&copy; Craighead Diocesan School 2024</p>
</footer>
