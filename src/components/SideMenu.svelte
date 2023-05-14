<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    export let wantHint;
    export let amountArray = [];
    export let done = false;
    let showToolTip = false;
</script>

<div class="w-auto flex justify-center items-center pb-3">
    <div class="w-full  flex-row flex  justify-center items-center rounded ">
        <label class="flex justify-end items-end mr-2 "
            >Hints:
            <input
                type="checkbox"
                class="ml-3 w-6 h-6"
                bind:checked={wantHint}
            />
        </label>
        <button
            on:click={() => dispatch("removeHints")}
            class="py-1 px-2  bg-blue-500 hover:bg-blue-400 rounded-lg text-white flex justify-center items-center flex-nowrap whitespace-nowrap"
        >
            <img
                src="src/assets/Hint.png"
                alt="settings"
                class="w-6 h-6 cursor-pointer"
            />
            <h1 class="ml-1">Delete only hints</h1></button
        >
        <button
            class="relative ml-2"
            on:mouseleave={() => (showToolTip = false)}
            on:mouseenter={() => (showToolTip = true)}
        >
            <img
                src="src/assets/info.png"
                alt="settings"
                class="w-6 h-6 cursor-pointer"
            />
            <div
                class="shadow p-2 whitespace-nowrap bg-white border rounded  top-6 left-0 {!showToolTip
                    ? 'hidden'
                    : 'absolute'}"
            >
                <table class="text-center min-w-full">
                    <thead>
                        <tr class="text-xl">
                            <th class="w-1/4 py-4 px-8">Wartość</th>
                            <th>Pozostała ilość</th>
                        </tr>
                    </thead>
                    <tbody class="divide-y-2">
                        {#each amountArray as el}
                            <tr class="divide-x-2">
                                <td>{el.value}</td>
                                <td>{el.amountLeft}</td>
                            </tr>
                        {/each}
                    </tbody>
                </table>
            </div>
        </button>
        <div
            class="flex justify-center items-center border border-blue-600 rounded-xl p-1 ml-2"
        >
            {#if !done}
                Rozwiąż zestaw
                <img
                    src="src/assets/try.png"
                    alt="settings"
                    class="w-8 h-8 cursor-pointer ml-1"
                />
            {:else}
                Rozwiązałeś zestaw
                <img
                    src="src/assets/trophy.png"
                    alt="settings"
                    class="w-8 h-8 cursor-pointer ml-1"
                />
            {/if}
        </div>
    </div>
</div>
