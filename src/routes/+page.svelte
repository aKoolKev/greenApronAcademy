
<script>
    import {holidayDrinks_2025_hot} from "$lib/data/drinks.js";

    let drinkName = '';
    
    //Generate random number between [0, max)
    function randNum (max) {
        return (Math.floor(Math.random() * max));
    }

    function getDrink(){
        drinkName = holidayDrinks_2025_hot[randNum(holidayDrinks_2025_hot.length)].name;
    }
</script>

<main class="text-white">
    <h1 class="font-bold uppercase text-3xl">Green Apron Academy</h1>
    <button on:click={getDrink}>Get Drink</button>
    <div>
        <span>{drinkName}</span>
    </div>
</main>

