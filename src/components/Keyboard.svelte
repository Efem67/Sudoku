<script>
    import { createEventDispatcher } from "svelte";
    export let keyRows;
    export let usedLetters;
    export let lettersOnGoodPos;
    export let lettersOnlyInWord;
    export let gameEnded;
    const dispatch = createEventDispatcher();

    const keyClick = (key) => {
        dispatch("keyboardClick", key.toUpperCase());
    };

    const originalKeyboardClick = (event) => {
        if (event.keyCode >= 65 && event.keyCode <= 90) {
            dispatch("keyboardClick", event.key.toUpperCase());
        } else if (event.keyCode == 13 || event.keyCode == 8) {
            dispatch("keyboardClick", event.key.toUpperCase());
        }
    };
    $: {
        if (gameEnded)
            window.removeEventListener("keydown", originalKeyboardClick);
    }
</script>

<svelte:window on:keydown={originalKeyboardClick} />
<div
    class="w-full h-2/6 flex flex-col items-center justify-center border-t border-gray-300 shadow-inner"
>
    {#each Object.entries(keyRows) as [title, row]}
        <div class="w-auto flex flex-row shadow-md rounded-md">
            {#each row as key}
                {#if lettersOnGoodPos.includes(key)}
                    <button
                        on:click={() => keyClick(key)}
                        class="w-auto p-4 m-1 mx-2 flex justify-center items-center font-bold rounded-md focus:ring-2 ring-blue-400
bg-green-400"
                    >
                        {key}
                    </button>
                {:else if lettersOnlyInWord.includes(key)}
                    <button
                        on:click={() => keyClick(key)}
                        class="w-auto p-4 m-1 flex justify-center items-center font-bold rounded-md focus:ring-2 ring-blue-400
bg-yellow-300"
                    >
                        {key}
                    </button>
                {:else if usedLetters.includes(key)}
                    <button
                        on:click={() => keyClick(key)}
                        class="w-auto p-4 m-1 flex justify-center items-center font-bold rounded-md focus:ring-2 ring-blue-400
bg-gray-400"
                    >
                        {key}
                    </button>
                {:else}
                    <button
                        on:click={() => keyClick(key)}
                        class="w-auto p-4 m-1 flex justify-center items-center font-bold rounded-md focus:ring-2 ring-blue-400
bg-gray-300"
                    >
                        {key}
                    </button>
                {/if}
            {/each}
        </div>
    {/each}
</div>
