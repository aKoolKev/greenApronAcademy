
<script>
    //grabbing all the drink datas
    import {allDrinks} from "$lib/data/drinks.js";
    import {coldFoams, espressos, milks, sauces, syrups, toppings} from "$lib/data/ingredients.js";

    //using flowbite svelte kit components
    import {Button, Input, P, Select, Label, Heading, Dropdown, DropdownItem, Card} from "flowbite-svelte";
    import {ChevronDownOutline} from "flowbite-svelte-icons";
    import {onMount} from "svelte";

    //global vars
    let randomDrink; //a random drink
    let drinkStep; // a random step of the random drink
    let drinkSize; // the size of the random drink

    //used to populate the user responses for certain drink step
    let buttonAmount = [1,2,3,4,5,6];
    let toppingAmountBtn = [1,2,3];
    let espressoAmountBtn = [1,2,3,4];
    

    //user's response to the current question

    let selectValue = ''; //for Select elements
    let userResponseResult = false; //indicate if user answered correct or not

    let multiPartQuestion = false; //if the question have several parts
    let hideNextDrinkBtn = true;


    //checks user response on Select Elements
    $: if (selectValue !== '') {

        if(drinkStep === "espresso") {
            if(selectValue === randomDrink.espresso)
                successMsg(true);
            else 
                successMsg(false);
        }
        else if (drinkStep === "coldFoam") {
            if (selectValue === randomDrink[drinkStep]) 
                successMsg(true);
            else 
                successMsg(false);
        }
        else if (drinkStep === "sauce" || "syrup" || "topping") {
            multiPartQuestion = true;
            if (selectValue === randomDrink[drinkStep]) 
                successMsg(true);
            else 
                successMsg(false);
        }
    }

    //check user response when selecting with number valued buttons
    function checkAmount(userValue){
        //translate size text into corresponding index
        let index;

        if (randomDrink.isHot) {
            index=0; //Short
            if (drinkSize === "Tall") index=1;
            else if (drinkSize === "Grande") index=2;
            else if (drinkSize === "Venti") index=3;
        } else {
            index = 0; // Tall
            if (drinkSize === "Grande") index=1;
            else if (drinkSize === "Venti") index=2;
        }

        //<drinkStep>Amount = #
            // toppingAmount
        if (drinkStep === "topping"){
            if (userValue === randomDrink[drinkStep+'Amount']) successMsg(true);
            else successMsg(false); 
        }
        //<drinkStep> = [] 
            //frapRoast
        else if (drinkStep === "frapRoast"){
            if (userValue == randomDrink[drinkStep][index]) successMsg(true);
            else successMsg(false);
        }
        else { // <drinkStep>Amount = [] 
            //espresso, sauce, syrup, syrupBaseAmount, concentrate
            if (userValue == randomDrink[drinkStep+'Amount'][index]) successMsg(true);
            else successMsg(false);
        }
       
    }
    
    //check user response when answering with text value buttons
    function checkValue(userValue){
        if (drinkStep === "syrupBase") multiPartQuestion = true;

        if(userValue === randomDrink[drinkStep]) successMsg(true);
            else successMsg(false);
    }


    //user response feedback
    function successMsg(val){
        if (val) { //user answered correctly
            alert("Good Job!");
            
            if(multiPartQuestion && !userResponseResult) {
                userResponseResult = true;
            } 
            else if (multiPartQuestion && userResponseResult) {
                hideNextDrinkBtn = false;
            } else {
                // not multi Part Question => Show "Next Drink" btn immediately
                hideNextDrinkBtn = false;
            }
        }
        else //user was incorrect
            alert("Try Again!");
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

        // get random drink size
        getRandDrinkSize();

        // pick random drink step to quiz
        drinkStep = randomDrink.steps[getRandNum(randomDrink.steps.length)];

        // reset values for each drink 
        selectValue ='';
        userResponseResult = false;
        hideNextDrinkBtn = true;
        multiPartQuestion = false;
    }
</script>

<main class= "text-center text-[#edf5f2]">
    <h1 class="font-bold uppercase text-4xl m-10">Green Apron Academy</h1>

    <!-- Possible Feature: Menu of game modes or drinks types to practice -->

    <!-- brown: bg-[#3B3230] -->
    <Card class="p-4 sm:p-6 md:p-8 bg-[#946f56] mt-10 mx-auto shadow-lg shadow-black">

    
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
                    <Heading tag="h5" class="m-2">Select Espresso Type</Heading>
                    <Select class="mt-2"
                            items={espressos}
                            bind:value={selectValue}
                    />
                </Label>
                
                <div class="m-5">
                    {#if userResponseResult}
                    <Heading tag="h5" class="m-2">Espresso Amount</Heading>
                    <div>
                        {#each espressoAmountBtn as value}
                            <Button onclick={()=>checkAmount(value)} class="mr-1 mt-1 bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2]">{value}</Button>
                        {/each}
                    </div>
                    {/if}
                </div>

                {:else if drinkStep === "milk"}
                    <Heading tag="h5" class="m-2 text-white">Select Milk</Heading>
                    <div class="m-2">
                        {#each milks as milk}
                            <Button color="dark" class="mr-1 mt-1 bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2]" onclick={()=>checkValue(milk.value)} >{milk.name}</Button>
                        {/each}
                    </div>
                {:else if drinkStep === "sauce"}
                    <Label>
                        <Heading tag="h5" class="m-2 text-white">Select Sauce</Heading>
                        <Select class="mt-2" items={sauces} bind:value={selectValue} />
                    </Label>

                    <div class="m-5">
                        {#if userResponseResult}
                            <Heading tag="h5" class="m-2 text-white">Sauce Pump Amount</Heading>
                            <div>
                                {#each buttonAmount as value}
                                    <Button color="dark" onclick={()=>checkAmount(value)} class="mr-1 mt-1 bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2]">{value}</Button>
                                {/each}
                            </div>
                        {/if}
                    </div>
                {:else if drinkStep === "syrup"}
                    <Label>
                        <Heading tag="h5" class="m-2 text-white">Select Syrup</Heading>
                        <Select class="mt-2" items={syrups} bind:value={selectValue} />
                    </Label>

                    <div class="m-5">
                        {#if userResponseResult}
                            <Heading tag="h5" class="m-2 text-white">Syrup Pumps Amount</Heading>
                            <div>
                                {#each buttonAmount as value}
                                    <Button color="dark" onclick={()=>checkAmount(value)} class="mr-1 mt-1 bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2]">{value}</Button>
                                {/each}
                            </div>
                        {/if}
                    </div>
                {:else if drinkStep === "whippedCream"}
                    <Heading tag="h5" class="m-2 text-white">Whipped Cream?</Heading>
                    <div class="m-2">
                        <Button class="bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2]" onclick={
                            ()=>{
                                if(true === randomDrink.whippedCream)
                                    successMsg(true);
                                else
                                    successMsg(false);
                            }
                        }>Yes</Button>
                        <Button class="bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2]" onclick={
                            (e)=>{
                                if(false === randomDrink.whippedCream)
                                    successMsg(true);
                                else
                                    successMsg(false);
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
                            {#if userResponseResult}
                                <Heading tag="h5" class="text-white m-2">
                                    Topping Amount
                                </Heading>

                                <div>
                                    {#each toppingAmountBtn as value}
                                        <Button class="bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2] mr-1 mt-1" onclick={()=>checkAmount(value)}>{value}</Button>
                                    {/each}
                                </div>
                            {/if}
                        </div>
                {:else if drinkStep === "syrupBase"}
                    <div>
                        <Heading tag="h5" class="m-2 text-white">Select Syrup Base</Heading>
                        <div>
                            <Button class="bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2]" onclick={()=>checkValue("Creme")}>Creme</Button>
                            <Button class="bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2]" onclick={()=>checkValue("Coffee")}>Coffee</Button>
                        </div>
                    </div>

                    <div class="m-5">
                        {#if userResponseResult}
                            <Heading tag="h5" class="m-2 text-white">Syrup Base Amount</Heading>
                            <div>
                                {#each buttonAmount as value}
                                    <Button onclick={()=>checkAmount(value)} class="mr-1 mt-1 bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2]">{value}</Button>
                                {/each}
                            </div>
                        {/if}
                    </div>
                {:else if drinkStep === "concentrate"}
                    <div class="m-2">
                        <Heading tag="h5" class="m-2 text-white">Select {randomDrink.concentrate} Amount</Heading>
                        <div>
                            {#each buttonAmount as value}
                                <Button color="dark" onclick={()=>checkAmount(value)} class="mr-1 mt-1 bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2]">{value}</Button>
                            {/each}
                        </div>
                    </div>
                {:else if drinkStep === "frapRoast"}
                    <div class="m-2">
                        <Heading tag="h5" class="m-2 text-white">Select Frap Roast Amount</Heading>
                        <div>
                            {#each buttonAmount as value}
                                <Button color="dark" onclick={()=>checkAmount(value)} class="mr-1 mt-1 bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2]">{value}</Button>
                            {/each}
                        </div>
                    </div>
                {:else if drinkStep === "hotWater"}
                    <div>
                        <Heading tag="h5" class="m-2 text-white">Select Hot Water Amount</Heading>
                        <Button class="bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2] mr-1 mt-1" value="1/2" onclick={
                            (e)=>{
                                if(e.target.value === randomDrink.hotWaterAmount) successMsg(true);
                                else successMsg(false);
                            }
                        }>1/2 cup</Button>
                        <Button class="bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2] mr-1 mt-1" value="Full" onclick={
                            (e)=>{
                                if(e.target.value === randomDrink.hotWaterAmount) successMsg(true);
                                else successMsg(false);
                            }
                        }>Full cup</Button>
                    </div>
                {:else if drinkStep === "milkAmount"}
                    <div>
                        <Heading tag="h5" class="m-2 text-white">Select Milk Amount</Heading>
                        <Button class="bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2] mr-1 mt-1" value="1/2" onclick={
                            (e)=>{
                                if (e.target.value === randomDrink.milkAmount) successMsg(true);
                                else successMsg(false);
                            }
                        }>1/2 cup</Button>
                        <Button class="bg-[#50af63] text-[#3B3230] text-md font-bold hover:bg-[#edf5f2] mr-1 mt-1" value="Full" onclick={
                            (e)=>{
                                if (e.target.value === randomDrink.milkAmount) successMsg(true);
                                else successMsg(false);
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
    
        {#if !hideNextDrinkBtn}
            <Button onclick={getDrink} class="m-5 w-fit mx-auto bg-[#50af63] hover:bg-[#edf5f2] text-black animatation animate-pulse">Next Drink</Button>
        {/if}

        <!-- <Button onclick={checkResponse} class="m-5 w-fit mx-auto" color="red">Check</Button> -->
    <!-- </div> -->
    </Card>    
    

</main>
