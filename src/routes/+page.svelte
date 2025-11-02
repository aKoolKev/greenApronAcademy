
<script>
    import {allDrinks} from "$lib/data/drinks.js";
    import {Button} from "flowbite-svelte";

    let drinkName = '';
    
    //Generate random number between [0, max)
    function randNum (max) {
        return (Math.floor(Math.random() * max));
    }

    function getDrink(){
        drinkName = allDrinks[randNum(allDrinks.length)].name;
    }
</script>

<main class="text-white text-center">
    <h1 class="font-bold uppercase text-4xl">Green Apron Academy</h1>

    <div class="flex flex-col mt-15 border border-white w-1/2 mx-auto rounded-xl p-5">
        <span class="font-bold text-lg">{drinkName}</span>
        <Button onclick={getDrink} class="m-5 w-fit mx-auto" color="red">Get Drink</Button>
    </div>

</main>

