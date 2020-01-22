<script>
    import { onMount } from 'svelte';

    let intervallOptions = [
        "once",
        "weekly",
        "monthly",
        "quarterly",
        "yearly"
    ];

    let abos = [
        {name: "Netflix", price: 11.99, intervall: intervallOptions[2], isAddAbo: false, isIncome: false, dropdown: ''},
        {name: "Spotify", price: 9.99, intervall: intervallOptions[2], isAddAbo: false, isIncome: true, dropdown: ''},
        {name: "", price: "", intervall: intervallOptions[0], isAddAbo: true, isIncome: false, dropdown: ''}
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

    // dropdown logic
    let dropdownStatus = [];
    let isDropdownActive = false;

    function toggleIsActive(index) {
        dropdownStatus.forEach((el, i) => {
            el.classList.remove("is-active");
            if (index == i && !isDropdownActive) {
                el.classList.add("is-active");
                isDropdownActive = true;
            } else if (index == i) {
                isDropdownActive = false;
            }
        });
        dropdownStatus = dropdownStatus;
    }

    function itemClicked(index, intervall) {
        abos[index].intervall = intervall;
        toggleIsActive(index);
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

.level-item .is-danger {
    border-color: whitesmoke;
    color: rgb(253, 116, 116);
}

.level-item .is-primary {
    border-color: whitesmoke;
}

.level-item .switch[type="checkbox"].is-warning.is-outlined:checked + label::after, .switch[type="checkbox"].is-warning.is-outlined:checked + label:after  { 
    background-color: whitesmoke !important;
}

.level-item .switch[type="checkbox"].is-warning.is-outlined:checked + label::before, .switch[type="checkbox"].is-warning.is-outlined:checked + label:before {
    border-color: whitesmoke !important;
}

.level-item .is-danger:hover {
    background-color: rgb(253, 116, 116);
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

.dropdown button {
    border: none !important;
    min-width: 110px !important;
}


</style>

{#each abos as abo, i}
<div class="box" 
    class:has-background-link={abo.isIncome} 
    class:has-background-danger={!abo.isIncome}
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
                            class:has-background-info={abo.isIncome}
                            class:expense-secondary={!abo.isIncome}
                            type="text" 
                            placeholder="Abonnement"
                            >
                    </p>  
                </div>
            </div>
            <!-- abo text -->
            <div class="level-item">
                <p class="subtitle is-5 has-text-light">
                    {#if abo.isIncome}
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
                            class:has-background-info={abo.isIncome}
                            class:expense-secondary={!abo.isIncome}
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
                        <!-- <div class="select is-white">
                            <select name="intervall"
                                class:has-background-info={abo.isIncome}
                                class:expense-secondary={!abo.isIncome}
                                >
                                {#each intervallOptions as intervall}
                                    {#if abo.intervall === intervall}
                                    <option selected>{intervall}</option>
                                    {:else}
                                    <option>{intervall}</option>
                                    {/if}
                                {/each}
                            </select>
                        </div> -->
                        
                        <div class="dropdown" bind:this={dropdownStatus[i]} >
                            <div class="dropdown-trigger">
                                <button 
                                    class="button has-text-light"
                                    class:has-background-info={abo.isIncome}
                                    class:expense-secondary={!abo.isIncome}
                                    on:click="{() => toggleIsActive(i)}" aria-haspopup="true" aria-controls="dropdown-menu">
                                    <span>{abo.intervall}</span>
                                    <span class="icon is-small">
                                        <i class="fas fa-angle-down" aria-hidden="true"></i>
                                    </span>
                                </button>
                            </div>
                            
                            <div class="dropdown-menu" id="dropdown-menu" role="menu">
                                <div class="dropdown-content">
                                    {#each intervallOptions as intervall}
                                    <a on:click="{() => itemClicked(i, intervall)}" class="dropdown-item">
                                        {intervall}
                                    </a>
                                    <hr class="dropdown-divider">
                                    {/each}
                                </div>
                            </div>
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
                        class="switch is-warning is-outlined" 
                        on:click="{() => toggleType(abo)}"
                        checked="checked">
                        <label for="toggle-type"></label>
                    
                </div>
            </div>
            {:else}
            <p class="level-item">
                <button class="button is-primary has-text-white is-outlined">
                    <span class="icon is-small">
                        <i class="fas fa-check"></i>
                    </span>
                </button>
            </p>
            {/if}
            <p class="level-item">
                {#if abo.isAddAbo}
                <button class="button is-primary has-text-white is-outlined" on:click="{add}">
                    <span class="icon is-small">
                        <i class="fas fa-plus"></i>
                    </span>
                </button>
                {:else}
                <button class="button is-danger has-text-white is-outlined" on:click="{() => remove(abo)}">
                    <span class="icon is-small">
                        <i class="fas fa-trash"></i>
                    </span>
                </button>
                {/if}
            </p>
        </div>
    </div>
</div>
{/each}

