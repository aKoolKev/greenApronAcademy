
<script>
    import {allDrinks} from "$lib/data/drinks.js";
    import {Button, Input, P} from "flowbite-svelte";
    import {onMount} from "svelte";

    let drinkStep = '';
    let randomDrink;

    onMount( ()=>{
        getDrink();
    });

    function getRandDrinkSize(){
        if (!randomDrink) return '';

        let randNum;
        if (randomDrink.isHot)
        {
            randNum = getRandNum(4);
            switch (randNum) {
                case 0: return "Short";
                case 1: return "Tall";
                case 2: return "Grande";
                case 3: return "Venti";
            }
        }
        else { //cold drink
            randNum = getRandNum(3);
            switch (randNum) {
                case 0: return "Tall";
                case 1: return "Grande";
                case 2: return "Venti";
            }
        }
    }
    
    //Generate random number between [0, max)
    function getRandNum (max) {
        return (Math.floor(Math.random() * max));
    }

    function getDrink(){
        //display drink's name
        randomDrink = allDrinks[getRandNum(allDrinks.length)];

        //TO-DO: pick random step to quiz
        drinkStep = randomDrink.steps[getRandNum(randomDrink.steps.length)];
        
        //TO-DO: show response options

        //TO-DO: check user response

    }
</script>

<main class="text-white text-center">
    <h1 class="font-bold uppercase text-4xl">Green Apron Academy</h1>

    <!-- Possible Feature: Menu of game modes or drinks types to practice -->

    <div class="flex flex-col mt-15 border border-white w-1/2 mx-auto rounded-xl p-5">
        
        <!-- component renders on ssr first, then gets hydrated in browser. prevent component access before randomDrink is "hydrated"  -->
        {#if randomDrink}
            <span class="font-bold text-lg">{randomDrink.name}</span>
            <span class="uppercase">{drinkStep}</span>

            {#if drinkStep==="espresso"}
                <!-- Pick random size -->
                <p>Size: <span>{getRandDrinkSize()}</span></p>
                <div class="m-2">
                    <Button>1</Button>
                    <Button>2</Button>
                    <Button>3</Button>
                    <Button>4</Button>
                </div>
                {:else if drinkStep === "milk"}
                    <div class="m-2">
                        <Button>2%</Button>
                        <Button>Whole</Button>
                        <Button>Oat</Button>
                    </div>
                {:else if drinkStep === "sauce" || drinkStep === "syrup"}
                    <Input placeholder="Enter {drinkStep}" size="sm"/>

                    <p class="mt-2 uppercase"> {drinkStep} Pumps: </p>
                    <div>
                        <Button>2</Button>
                        <Button>3</Button>
                        <Button>4</Button>
                        <Button>5</Button>
                        <Button>6</Button>
                    </div>
                {:else if drinkStep === "whippedCream"}
                    <h2> Whipped Cream? </h2>
                    <div class="m-2">
                        <Button>Yes</Button>
                        <Button>No</Button>
                    </div>
            {/if}


        {/if}
       

        <Button onclick={getDrink} class="m-5 w-fit mx-auto" color="red">Get Drink</Button>
    </div>

</main>

