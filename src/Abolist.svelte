<script>
    import 'bulma/css/bulma.css';
    import 'bulma-switch/dist/css/bulma-switch.min.css';

    let intervallOptions = [
        "once",
        "weekly",
        "monthly",
        "quarterly",
        "yearly"
    ];

    let abos = [
        {name: "Netflix", price: 11.99, intervall: intervallOptions[2], isAddAbo: false, isIncome: false},
        {name: "Spotify", price: 9.99, intervall: intervallOptions[2], isAddAbo: false, isIncome: true},
        {name: "", price: "", intervall: intervallOptions[0], isAddAbo: true, isIncome: false}
    ];

    function add() {
        abos[abos.length -1].isAddAbo = false;
        abos = [...abos, {name: "", price: "", intervall: intervallOptions[0], isAddAbo: true, isIncome: false}];
    }

    function remove(abo) {
        abos = abos.filter((a => a !== abo));
    }

    function toggleType(abo) {
        abo.isIncome = !abo.isIncome;
        abos = abos;
    }

</script>

<style>

input {
    border: none;
    outline-style:none !important;
    box-shadow:none !important;
    border-color:transparent !important;
}

::placeholder {
    color: hsl(0, 0%, 86%) !important;
}

select {
    border: none !important;
    color: hsl(0, 0%, 86%) !important;
}

.select.is-white:after {
  border-color: hsl(0, 0%, 86%) !important;
}



.expense-secondary {
    background-color: hsl(355, 78%, 55%) !important;
    border: none !important;
}

.expense-secondary:hover {
    background-color: hsl(355, 68%, 52%) !important;
}

.expense-secondary:active {
    background-color: hsl(355, 64%, 45%) !important;
}

</style>


{#each abos as abo}
<div class="columns is-centered">
    <!-- column abonements -->
    <div class="column is-7">
        <div class="box" 
            class:has-background-link={abo.isAddAbo} 
            class:has-background-danger={!abo.isAddAbo}
            >
            <!-- level -->
            <div class="level">
                <!-- left -->
                <div class="level-left">
                    <!-- text field abo-->
                    <div class="level-item">
                        <div class="field">
                            <p class="control">
                                <input value={abo.name} 
                                    class="input has-text-grey-lighter" 
                                    class:has-background-info={abo.isAddAbo}
                                    class:expense-secondary={!abo.isAddAbo}
                                    type="text" 
                                    placeholder="Abonnement"
                                    >
                            </p>  
                        </div>
                    </div>
                    <!-- abo text -->
                    <div class="level-item">
                        <p class="subtitle is-5 has-text-light">
                            {#if abo.isAddAbo}
                            earns you
                            {:else}
                            costs you 
                            {/if}
                        </p>  
                    </div>
                    <!-- text field price -->
                    <div class="level-item">
                        <div class="field">
                            <p class="control">
                                <input value={abo.price} 
                                    class="input has-text-grey-lighter"
                                    class:has-background-info={abo.isAddAbo}
                                    class:expense-secondary={!abo.isAddAbo}
                                    type="text" 
                                    placeholder="amount">
                            </p>  
                        </div>
                    </div>
                    <!-- € text -->
                    <div class="level-item">
                        <p class="subtitle is-5 has-text-light">
                            €
                        </p>  
                    </div>
                    <!-- text field intervall -->
                    <div class="level-item">
                        <div class="field">
                            <div class="control">
                                <div class="select is-white">
                                    <select name="intervall"
                                        class:has-background-info={abo.isAddAbo}
                                        class:expense-secondary={!abo.isAddAbo}
                                        >
                                        {#each intervallOptions as intervall}
                                            {#if abo.intervall === intervall}
                                            <option selected>{intervall}</option>
                                            {:else}
                                            <option>{intervall}</option>
                                            {/if}
                                        {/each}
                                    </select>
                                </div>
                            </div>  
                        </div>
                    </div>
                </div>
                <!-- right -->
                <!-- save / income - expense buttons -->
                <div class="level-right">
                    {#if abo.isAddAbo}
                    <div class="level-item">
                        <div class="field">
                            <input id="toggle-type" 
                                type="checkbox" 
                                name="toggle-type" 
                                class="switch is-rtl is-rounded" 
                                class:is-info={abo.income}
                                class:expense-secondary={!abo.income}
                                on:click="{() => toggleType(abo)}"
                                checked="checked">
                            {#if abo.isIncome}
                            <label for="toggle-type" class="subtitle is-5 has-text-white">Income</label>
                            {:else}
                            <label for="toggle-type" class="subtitle is-5 has-text-white">Expense</label>
                            {/if}
                        </div>
                    </div>
                    {:else}
                    <p class="level-item">
                        <button class="button is-primary has-text-white">Save</button>
                    </p>
                    {/if}
                    <p class="level-item">
                        {#if abo.isAddAbo}
                        <button class="button is-info has-text-white" on:click="{add}">Add</button>
                        {:else}
                        <button class="button expense-secondary has-text-white" on:click="{() => remove(abo)}">Delete</button>
                        {/if}
                    </p>
                </div>
            </div>
        </div>
    </div>
</div>
{/each}