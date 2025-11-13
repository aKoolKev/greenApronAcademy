
<script>
    import {allDrinks} from "$lib/data/drinks.js";
    import {Button, Input, P, Select, Label, Heading, Dropdown, DropdownItem, select, Card} from "flowbite-svelte";
    import {ChevronDownOutline} from "flowbite-svelte-icons";
    import {onMount} from "svelte";

    let drinkStep = '';
    let randomDrink;
    let drinkSize;

    let buttonAmount = [1,2,3,4,5,6];
    let toppingAmountBtn = [1,2,3];
    let espressoAmountBtn = [1,2,3,4];
    

    //user's response to the current question
    let userResponse = "None";
    let selectValue = '';

    let coldFoams = [
        {value:"Gingerbread Cream", name:"Gingerbread"},
        {value:"Vanilla Sweet Cream", name:"Vanilla Sweet Cream"},
        {value:"Chocolate Cream", name:"Chocolate"},
        {value:"Pumpkin Spice Cream", name:"Pumpkin Spice"},
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
        {value:"White Chocolate Mocha", name:"White Chocolate Mocha"},
        {value:"Caramel Brulee", name:"Caramel Brulee"},
        {value:"Pumpkin Spice", name:"Pumpkin Spice"}
    ]

    let toppings = [
        { value: "Chocolate Curls", name: "Chocolate Curls" },
        { value: "Caramel", name: "Caramel" },
        { value: "Caramel Brulee", name: "Caramel Brulee" },
        { value: "Green and Red Sprinkles", name: "Green and Red Sprinkles" },
        { value: "Pecan Crunch", name: "Pecan crunch" },
        { value: "Pumpkin Spice", name: "Pumpkin Spice" },
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

    $: if (selectValue!='') {
        if(drinkStep === "espresso") {
            if(selectValue === randomDrink.espresso)
                alert("Correct!");
            else 
                alert("Incorrect!");
        }
        else if (drinkStep === "sauce" || "syrup" || "toppings") {
            if (selectValue === randomDrink[drinkStep]) 
                alert("Correct!");
            else 
                alert ("Incorrect!");
        }
    }

    function checkAmount(userValue){
        //translate size text into corresponding index
        let index=0;
        if (drinkSize === "Grande") index=1;
        else if (drinkSize === "Venti") index=2;

        if (Number.isInteger(randomDrink[drinkStep][index])) {
            if (userValue == randomDrink[drinkStep][index]) alert("Correct!");
            else alert ("Incorrect!");
        }
        else { //must handle differently by selecting "<drinkStep>Amount"
            if (userValue == randomDrink[drinkStep+'Amount'][index]) alert("Correct!");
            else alert ("Incorrect!");
        }
       
    }

    function checkValue(userValue){
        if(userValue === randomDrink[drinkStep]) alert("Correct!");
        else alert("Incorrect!");
    }

    function checkEspressoAmount(){
        let index; 

        //translate size into respective index
        if (drinkSize === "Tall")
            index = 0;
        else if (drinkSize === "Grande")
            index = 1;
        else 
            index = 2;

        //check and handle user's response
        alert(randomDrink.espressoAmount[index]);
        if (userResponse == randomDrink.espressoAmount[index])
            alert ("Correct!");
        else 
            alert ("Incorrect!");
    } 

    // get and display a random drink when page hydrates
    onMount( ()=>{
        getDrink();
    });

    //generate a random drink size based on if it is an hot or cold drink
    function getRandDrinkSize(){
        if (!randomDrink) return '';

        let randNum;
        if (randomDrink.isHot)
        {
            randNum = getRandNum(4);
            switch (randNum) {
                case 0: drinkSize = "Short"; break;
                case 1: drinkSize = "Tall"; break;
                case 2: drinkSize = "Grande"; break;
                case 3: drinkSize = "Venti"; break;
            }
        }
        else { //cold drink
            randNum = getRandNum(3);
            switch (randNum) {
                case 0: drinkSize = "Tall"; break;
                case 1: drinkSize = "Grande"; break;
                case 2: drinkSize = "Venti"; break;
            }
        }
    }
    
    //Generate random number between [0, max)
    function getRandNum (max) {
        return (Math.floor(Math.random() * max));
    }

    //select and display a random drink
    function getDrink(){
        //display drink's name
        randomDrink = allDrinks[getRandNum(allDrinks.length)];
        getRandDrinkSize();
        //TO-DO: pick random step to quiz
        drinkStep = randomDrink.steps[getRandNum(randomDrink.steps.length)];
        selectValue='';
        //TO-DO: check user response
    }
</script>

<main class="text-white text-center">
    <h1 class="font-bold uppercase text-4xl m-10">Green Apron Academy</h1>

    <!-- Possible Feature: Menu of game modes or drinks types to practice -->

    <Card class="p-4 sm:p-6 md:p-8 bg-[#3B3230] mt-10 mx-auto shadow-lg shadow-black">

    
    <!-- <div class="flex flex-col mt-15 border border-white w-3/4 mx-auto rounded-xl p-5"> -->
        
        <!-- component renders on ssr first, then gets hydrated in browser. prevent component access before randomDrink is "hydrated"  -->
        {#if randomDrink}
            
            <!-- DRINK NAME -->
            <span class="font-bold text-2xl mb-5">{randomDrink.name}</span>

            <!-- RANDOM DRINK SIZE -->
            <span class="text-lg italic font-bold">({drinkSize})</span>
            <hr class="mt-2 mb-5">

            <!-- Debug -->
            <!-- <span class="uppercase">{drinkStep}</span> -->

            {#if drinkStep==="espresso"}
                <!-- espresso type -->
                <Label>
                    <Heading tag="h5" class="m-2 text-white">Select Espresso Type</Heading>
                    <Select class="mt-2"
                            items={espressos}
                            bind:value={selectValue}
                    />
                </Label>
                
                <div class="m-5">
                    <Heading tag="h5" class="m-2 text-white">Espresso Amount</Heading>
                    <div>
                        {#each espressoAmountBtn as value}
                            <Button onclick={()=>checkAmount(value)} class="mr-1 mt-1 bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50">{value}</Button>
                        {/each}
                    </div>
                </div>
                {:else if drinkStep === "milk"}
                    <Heading tag="h5" class="m-2 text-white">Select Milk</Heading>
                    <div class="m-2">
                        {#each milks as milk}
                            <Button color="dark" class="mr-1 mt-1 bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50" onclick={()=>checkValue(milk.value)} >{milk.name}</Button>
                        {/each}
                    </div>
                {:else if drinkStep === "sauce"}
                    <Label>
                        <Heading tag="h5" class="m-2 text-white">Select Sauce</Heading>
                        <Select class="mt-2" items={sauces} bind:value={selectValue} />
                    </Label>

                    <div class="m-5">
                        <Heading tag="h5" class="m-2 text-white">Sauce Pump Amount</Heading>
                        <div>
                            {#each buttonAmount as value}
                                <Button color="dark" onclick={()=>checkAmount(value)} class="mr-1 mt-1 bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50">{value}</Button>
                            {/each}
                        </div>
                    </div>
                {:else if drinkStep === "syrup"}
                    <Label>
                        <Heading tag="h5" class="m-2 text-white">Select Syrup</Heading>
                        <Select class="mt-2" items={syrups} bind:value={selectValue} />
                    </Label>

                    <div class="m-5">
                        <Heading tag="h5" class="m-2 text-white">Syrup Pumps Amount</Heading>
                        <div>
                            {#each buttonAmount as value}
                                <Button color="dark" onclick={()=>checkAmount(value)} class="mr-1 mt-1 bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50">{value}</Button>
                            {/each}
                        </div>
                    </div>
                {:else if drinkStep === "whippedCream"}
                    <Heading tag="h5" class="m-2 text-white">Whipped Cream?</Heading>
                    <div class="m-2">
                        <Button class="bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50" onclick={
                            ()=>{
                                if(true === randomDrink.whippedCream)
                                    alert("Correct!");
                                else
                                    alert("Incorrect!");
                            }
                        }>Yes</Button>
                        <Button class="bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50" onclick={
                            (e)=>{
                                if(false === randomDrink.whippedCream)
                                    alert("Correct!");
                                else
                                    alert("Incorrect!");
                            }
                        }>No</Button>
                    </div>
                {:else if drinkStep === "topping"}
                    <Label>
                        <Heading tag="h5" class="text-white">
                            Select Topping
                        </Heading>
                        <Select class="mt-2" items={toppings} bind:value={selectValue} />
                    </Label>

                    <div class="m-5">
                        <Heading tag="h5" class="text-white m-2">
                            Topping Amount
                        </Heading>

                        <div>
                            {#each toppingAmountBtn as value}
                                <Button class="bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50 mr-1 mt-1" onclick={()=>checkAmount(value)}>{value}</Button>
                            {/each}
                        </div>
                    </div>
                {:else if drinkStep === "syrupBase"}
                    <div>
                        <Heading tag="h5" class="m-2 text-white">Select Syrup Base</Heading>
                        <div>
                            <Button class="bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50" onclick={()=>checkValue("Creme")}>Creme</Button>
                            <Button class="bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50" onclick={()=>checkValue("Coffee")}>Coffee</Button>
                        </div>
                    </div>
                        
                    <div class="m-5">
                        <Heading tag="h5" class="m-2 text-white">Syrup Base Amount</Heading>
                        <div>
                            {#each buttonAmount as value}
                                <Button color="dark" onclick={()=>checkAmount(value)} class="mr-1 mt-1 bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50">{value}</Button>
                            {/each}
                        </div>
                    </div>
                {:else if drinkStep === "concentrate"}
                    <div class="m-2">
                        <Heading tag="h5" class="m-2 text-white">Select {randomDrink.concentrate} Amount</Heading>
                        <div>
                            {#each buttonAmount as value}
                                <Button color="dark" onclick={()=>checkAmount(value)} class="mr-1 mt-1 bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50">{value}</Button>
                            {/each}
                        </div>
                    </div>
                {:else if drinkStep === "frapRoast"}
                    <div class="m-2">
                        <Heading tag="h5" class="m-2 text-white">Select Frap Roast Amount</Heading>
                        <div>
                            {#each buttonAmount as value}
                                <Button color="dark" onclick={()=>checkAmount(value)} class="mr-1 mt-1 bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50">{value}</Button>
                            {/each}
                        </div>
                    </div>
                {:else if drinkStep === "hotWater"}
                    <div>
                        <Heading tag="h5" class="m-2 text-white">Select Hot Water Amount</Heading>
                        <Button class="bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50 mr-1 mt-1" value="1/2" onclick={
                            (e)=>{
                                if(e.target.value === randomDrink.hotWaterAmount) alert("Correct!")
                                else alert("Incorrect!");
                            }
                        }>1/2 cup</Button>
                        <Button class="bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50 mr-1 mt-1" value="Full" onclick={
                            (e)=>{
                                if(e.target.value === randomDrink.hotWaterAmount) alert("Correct!")
                                else alert("Incorrect!");
                            }
                        }>Full cup</Button>
                    </div>
                {:else if drinkStep === "milkAmount"}
                    <div>
                        <Heading tag="h5" class="m-2 text-white">Select Milk Amount</Heading>
                        <Button class="bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50 mr-1 mt-1" value="1/2" onclick={
                            (e)=>{
                                if (e.target.value === randomDrink.milkAmount) alert ("Correct!");
                                else alert("Incorrect!");
                            }
                        }>1/2 cup</Button>
                        <Button class="bg-[#CCAE88] text-[#3B3230] text-md font-bold hover:bg-amber-50 mr-1 mt-1" value="Full" onclick={
                            (e)=>{
                                if (e.target.value === randomDrink.milkAmount) alert ("Correct!");
                                else alert("Incorrect!");
                            }
                        }>Full cup</Button>
                    </div>
                {:else if drinkStep === "coldFoam"}
                    <Label>
                        <Heading tag="h5" class="m-2 text-white">Select Cold Foam</Heading>
                        <Select class="mt-2" items={coldFoams} bind:value={selectValue} />
                    </Label>
                {:else}
                    <h1>{drinkStep}</h1>
                    {alert(drinkStep)}
            {/if}


        {/if}
    

        <Button onclick={getDrink} class="m-5 w-fit mx-auto" color="red">Get Drink</Button>
        <!-- <Button onclick={checkResponse} class="m-5 w-fit mx-auto" color="red">Check</Button> -->
    <!-- </div> -->
    </Card>    
    

</main>

<!-- syrup, sauce, chai, topping, espresso -->

