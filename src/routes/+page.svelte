<script>
  import Header from "$lib/Header.svelte";
  let carsForHire = [
    {
      name: "fast",
      cars: [
        {
          car: "Black Lamborghini",
          description:
            "A sleek, high-performance sports car in a striking black finish.",
          price: 2000,
          img: "https://digitech.craighead.school.nz/car-hire-images/black-lamborghini.jpg",
          credit: "Adrian N (Unsplash)",
          available: false,
        },
        {
          car: "Orange Lamborghini",
          description:
            "An eye-catching orange Lamborghini with top-tier speed and luxury.",
          price: 2000,
          img: "https://digitech.craighead.school.nz/car-hire-images/orange-lamborghini.jpg",
          credit: "Toni Zaat (Unsplash)",
          available: true,
        },
        {
          car: "Grey Aston Martin",
          description:
            "A sophisticated grey Aston Martin combining elegance with power.",
          price: 1500,
          img: "https://digitech.craighead.school.nz/car-hire-images/grey-aston-martin.jpg",
          credit: "Alexandru Ivanov (Unsplash)",
          available: true,
        },
        {
          car: "White Bugatti",
          description:
            "A luxurious white Bugatti known for its exceptional speed and design.",
          price: 3500,
          img: "https://digitech.craighead.school.nz/car-hire-images/white-bugatti.jpg",
          credit: "Flavien (username) (Unsplash)",
          available: true,
        },
        {
          car: "Red Ferrari",
          description:
            "A stunning red Ferrari that promises an exhilarating driving experience.",
          price: 1500,
          img: "https://digitech.craighead.school.nz/car-hire-images/red-ferrari.jpg",
          credit: "Molim Karbelaei (Unsplash)",
          available: true,
        },
      ],
    },
    {
      name: "nice",
      cars: [
        {
          car: "Black Maserati",
          description:
            "A stylish black Maserati offering luxury and high performance.",
          price: 1500,
          img: "https://digitech.craighead.school.nz/car-hire-images/black-maserati.jpg",
          credit: "Krysztof Kubicki (Unsplash)",
          available: true,
        },
        {
          car: "Black Mercedes",
          description:
            "A classy black Mercedes known for its comfort and advanced features.",
          price: 1500,
          img: "https://digitech.craighead.school.nz/car-hire-images/black-mercedes.jpg",
          credit: "Martin Katler (Unsplash)",
          available: false,
        },
        {
          car: "Green Chevrolet",
          description:
            "A vibrant green Chevrolet that blends style with reliable performance.",
          price: 1000,
          img: "https://digitech.craighead.school.nz/car-hire-images/green-chevrolet.jpg",
          credit: "Markus Spiske (Unsplash)",
          available: true,
        },
        {
          car: "Orange Volks Wagon",
          description:
            "A charming orange Volkswagen with a retro appeal and modern features.",
          price: 500,
          img: "https://digitech.craighead.school.nz/car-hire-images/orange-volks-wagon.jpg",
          credit: "Oli Woodman (Unsplash)",
          available: true,
        },
        {
          car: "Black MG",
          description:
            "A classic black MG known for its elegant design and smooth ride.",
          price: 1000,
          img: "https://digitech.craighead.school.nz/car-hire-images/black-mg.jpg",
          credit: "Robin Vet (Unsplash)",
          available: true,
        },
      ],
    },
  ];
  // let selected = carsForHire.[0];
  let selected = "";

  // go to the getWalls function and it will send you back the array.
  let GettingCars = getCars();

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

<!-- {selected} -->
<select bind:value={selected}>
  {#each carsForHire as cars}
    <option value={cars}> {cars.name} </option>
  {/each}
</select>

<!-- While you're still waiting for the data from the other website, show a waiting message -->
{#await GettingCars}
  ...waiting

  <!-- once you get the data, do this stuff -->
{:then GettingCars}
  {#each GettingCars.GettingCars as wall}
    <!-- create a card for each item in the array (we called it 'wall') and send the info to the card component -->

    name={wall.car}
    alt={wall.car}
    src={wall.img}
    caption={wall.description}
    money={wall.price}
  {/each}
{/await}
<!-- {
  car: "Black Lamborghini",
  description:
    "A sleek, high-performance sports car in a striking black finish.",
  price: 2000,
  img: "https://digitech.craighead.school.nz/car-hire-images/black-lamborghini.jpg",
  credit: "Adrian N (Unsplash)",
  available: false,
}, -->

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
