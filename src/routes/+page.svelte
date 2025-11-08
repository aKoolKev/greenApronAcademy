
<script>
    import {allDrinks} from "$lib/data/drinks.js";
    import {Button, Input, P, Select, Label, Heading, Dropdown, DropdownItem} from "flowbite-svelte";
    import {ChevronDownOutline} from "flowbite-svelte-icons";
    import {onMount} from "svelte";

    let drinkStep = '';
    let randomDrink;

    let selected = "None";

    let coldFoams = [
        {value:"Gingerbread", name:"Gingerbread"},
        {value:"Vanilla Sweet Cream", name:"Vanilla Sweet Cream"},
        {value:"Chocolate", name:"Chocolate"},
        {value:"Pumpkin Spice", name:"Pumpkin Spice"},
    ]

    let espressos = [
        {value:"Default", name:"Default"},
        {value:"Blonde", name:"Blonde"},
        {value:"Ristretto", name:"Ristretto"},
        {value:"Blonde Ristretto", name:"Blonde Ristretto"}
    ]

    let milks = [
        {value: "2%", name: "2%"},
        {value: "Whole", name: "Whole"},
        {value: "Oat", name: "Oat"},
        {value: "Coconut", name: "Coconut"},
    ]

    let sauces = [
        {value:"Mocha", name:"Mocha"},
        {value:"White Mocha", name:"White Mocha"},
        {value:"Caramel Brulee", name:"Caramel Brulee"},
        {value:"Pumpkin Spice", name:"Pumpkin Spice"}
    ]

    let toppings = [
        { value: "Chocolate curls", name: "Chocolate curls" },
        { value: "Caramel", name: "Caramel" },
        { value: "Caramel brulee", name: "Caramel brulee" },
        { value: "Green and Red Sprinkles", name: "Green and Red Sprinkles" },
        { value: "Pecan crunch", name: "Pecan crunch" },
        { value: "Pumpkin spice", name: "Pumpkin spice" },
        { value: "Cinnamon", name: "Cinnamon" },
        { value: "None", name: "None" },
    ];

    let syrups = [
        { value: "Vanilla", name: "Vanilla" },
        { value: "Sugar-free Vanilla", name: "Sugar-free Vanilla" },
        { value: "Caramel", name: "Caramel" },
        { value: "Pecan", name: "Pecan" },
        { value: "Sugar Cookie", name: "Sugar Cookie" },
        { value: "Cinnamon Dolce", name: "Cinnamon Dolce" },
        { value: "Gingerbread", name: "Gingerbread" },
        { value: "Brown Sugar", name: "Brown Sugar" },
        { value: "Hazelnut", name: "Hazelnut" },
        { value: "Honey Blend", name: "Honey Blend" },
        { value: "Peppermint", name: "Peppermint" },
        { value: "Classic", name: "Classic" },
        { value: "None", name: "None" },
    ];

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

    <div class="flex flex-col mt-15 border border-white w-3/4 mx-auto rounded-xl p-5">
        
        <!-- component renders on ssr first, then gets hydrated in browser. prevent component access before randomDrink is "hydrated"  -->
        {#if randomDrink}
            
            <!-- DRINK NAME -->
            <span class="font-bold text-2xl mb-5">{randomDrink.name}</span>

            <!-- RANDOM DRINK SIZE -->
            <span class="text-lg italic font-bold">({getRandDrinkSize()})</span>
            <hr class="mt-2 mb-5">

            <!-- Debug -->
            <!-- <span class="uppercase">{drinkStep}</span> -->

            {#if drinkStep==="espresso"}
                <!-- espresso type -->
                <Label>
                    <Heading tag="h5" class="m-2 text-white">Select Espresso Type</Heading>
                    <Select class="mt-2" items={espressos} bind:value={selected} />
                </Label>
                
                <div class="m-5">
                    <Heading tag="h5" class="m-2 text-white">Espresso Amount</Heading>
                    <div>
                        <Button color="dark">1</Button>
                        <Button color="dark">2</Button>
                        <Button color="dark">3</Button>
                        <Button color="dark">4</Button>
                    </div>
                </div>
                {:else if drinkStep === "milk"}
                    <Heading tag="h5" class="m-2 text-white">Select Milk</Heading>
                    <div class="m-2">
                        {#each milks as milk}
                            <Button color="dark" class="mr-2">{milk.name}</Button>
                        {/each}
                    </div>
                {:else if drinkStep === "sauce"}
                    <Label>
                        <Heading tag="h5" class="m-2 text-white">Select Sauce</Heading>
                        <Select class="mt-2" items={sauces} bind:value={selected} />
                    </Label>

                    <div class="m-5">
                        <Heading tag="h5" class="m-2 text-white">Sauce Pump Amount</Heading>
                        <div>
                            <Button color="dark">2</Button>
                            <Button color="dark">3</Button>
                            <Button color="dark">4</Button>
                            <Button color="dark">5</Button>
                            <Button color="dark">6</Button>
                        </div>
                    </div>
                {:else if drinkStep === "syrup"}
                    <Label>
                        <Heading tag="h5" class="m-2 text-white">Select Syrup</Heading>
                        <Select class="mt-2" items={syrups} bind:value={selected} />
                    </Label>

                    <div class="m-5">
                        <Heading tag="h5" class="m-2 text-white">Syrup Pumps Amount</Heading>
                        <div>
                            <Button color="dark">2</Button>
                            <Button color="dark">3</Button>
                            <Button color="dark">4</Button>
                            <Button color="dark">5</Button>
                            <Button color="dark">6</Button>
                        </div>
                    </div>
                {:else if drinkStep === "whippedCream"}
                    <Heading tag="h5" class="m-2 text-white">Whipped Cream?</Heading>
                    <div class="m-2">
                        <Button color="dark">Yes</Button>
                        <Button color="dark">No</Button>
                    </div>
                {:else if drinkStep === "topping"}
                    <Label>
                        <Heading tag="h5" class="text-white">
                            Select Topping
                        </Heading>
                        <Select class="mt-2" items={toppings} bind:value={selected} />
                    </Label>

                    <div class="m-5">
                        <Heading tag="h5" class="text-white m-2">
                            Topping Amount
                        </Heading>

                        <div>
                            <Button color="dark">1</Button>
                            <Button color="dark">2</Button>
                            <Button color="dark">3</Button>
                        </div>
                    </div>
                {:else if drinkStep === "syrupBase"}
                    <div>
                        <Heading tag="h5" class="m-2 text-white">Select Syrup Base</Heading>
                        <div>
                            <Button color="dark">Creme</Button>
                            <Button color="dark">Coffee</Button>
                        </div>
                    </div>
                        
                    <div class="m-5">
                        <Heading tag="h5" class="m-2 text-white">Syrup Base Amount</Heading>
                        <div>
                            <Button color="dark">2</Button>
                            <Button color="dark">3</Button>
                            <Button color="dark">4</Button>
                            <Button color="dark">5</Button>
                            <Button color="dark">6</Button>
                        </div>
                    </div>
                {:else if drinkStep === "concentrate"}
                    <div class="m-2">
                        <Heading tag="h5" class="m-2 text-white">Select {randomDrink.concentrate} Amount</Heading>
                        <Button color="dark">2</Button>
                        <Button color="dark">3</Button>
                        <Button color="dark">4</Button>
                        <Button color="dark">5</Button>
                        <Button color="dark">6</Button>
                    </div>
                {:else if drinkStep === "frapRoast"}
                    <div class="m-2">
                        <Heading tag="h5" class="m-2 text-white">Select Frap Roast Amount</Heading>
                        <Button color="dark">2</Button>
                        <Button color="dark">3</Button>
                        <Button color="dark">4</Button>
                        <Button color="dark">5</Button>
                        <Button color="dark">6</Button>
                    </div>
                    
                {:else if drinkStep === "hotWater"}
                    <div>
                        <Heading tag="h5" class="m-2 text-white">Select Hot Water Amount</Heading>
                        <Button color="dark">1/2 cup</Button>
                        <Button color="dark">Full cup</Button>
                    </div>
                {:else if drinkStep === "milkAmount"}
                    <div>
                        <Heading tag="h5" class="m-2 text-white">Select Milk Amount</Heading>
                        <Button color="dark">1/2 cup</Button>
                        <Button color="dark">Full cup</Button>
                    </div>
                {:else if drinkStep === "coldFoam"}
                    <!-- espresso type -->
                    <Label>
                        <Heading tag="h5" class="m-2 text-white">Select Cold Foam</Heading>
                        <Select class="mt-2" items={coldFoams} bind:value={selected} />
                    </Label>
                {:else}
                    <h1>{drinkStep}</h1>
                    {alert(drinkStep)}
            {/if}


        {/if}
       

        <Button onclick={getDrink} class="m-5 w-fit mx-auto" color="red">Get Drink</Button>
    </div>

</main>

