<script>
    import SideMenu from "./SideMenu.svelte";
    import { createEventDispatcher } from "svelte";
    export let array = [];
    export let amountArray = [];
    export let doneSudoku = false;

    let wantHint = false;
    const dispatch = createEventDispatcher();

    function handleKeydown(event) {
        const key = event.key;
        const code = event.code;
        if (key.match(/[1-9]/g)) {
            if (!wantHint) dispatch("fillField", { name: "value", value: key });
            else dispatch("fillField", { name: "hint", value: key });
            return;
        }

        let currI,
            currJ = 0;

        array.forEach((row, i) => {
            row.forEach((value, j) => {
                if (value.currentSelected) {
                    currI = i;
                    currJ = j;
                }
            });
        });
        let newIndexes;

        if (code == "Backspace") {
            dispatch("removeFromField");
        }

        if (code == "ArrowUp") {
            if (currI == 0) return;
            newIndexes = { i: currI - 1, j: currJ };
        } else if (code == "ArrowDown") {
            if (currI == 8) return;
            newIndexes = { i: currI + 1, j: currJ };
        } else if (code == "ArrowLeft") {
            if (currJ == 0) return;
            newIndexes = { i: currI, j: currJ - 1 };
        } else if (code == "ArrowRight") {
            if (currJ == 8) return;
            newIndexes = { i: currI, j: currJ + 1 };
        } else return;

        dispatch("arrowsMoveEvent", {
            last: { i: currI, j: currJ },
            new: newIndexes,
        });
    }
</script>

<svelte:window on:keydown={handleKeydown} />
<div class="h-full flex justify-center items-center flex-col">
    <SideMenu
        done={doneSudoku}
        bind:wantHint
        on:removeHints={() => dispatch("removeHints")}
        {amountArray}
    />
    <div class=" h-auto flex flex-col items-center justify-start w-full ">
        <div class="overflow-y-auto border-b-2">
            {#each array as row, i}
                <div class=" flex flex-row border-r-2 ">
                    {#each row as el, j}
                        <div
                            class="w-16 h-16 border-2 border-r-0 border-b-0 {j %
                                2 ==
                            0
                                ? i % 2 == 0
                                    ? 'bg-blue-100'
                                    : ''
                                : i % 2 == 1
                                ? 'bg-blue-100'
                                : ''}  flex items-center justify-center text-2xl font-bold {el.currentSelected
                                ? 'border-4 border-r-4 border-b-4 border-blue-800 border-dashed'
                                : ''}"
                        >
                            {#if el.content.value != 0}
                                <span
                                    class="{el.content.correct
                                        ? 'text-green-600'
                                        : 'text-red-600'}{el.solid
                                        ? 'text-black'
                                        : ''}"
                                    >{el.content.value == 0
                                        ? ""
                                        : el.content.value}</span
                                >
                            {:else}
                                <span class="text-blue-300 text-sm"
                                    >{el.content.hint.length == 0
                                        ? ""
                                        : el.content.hint}</span
                                >
                            {/if}
                        </div>
                    {/each}
                </div>
            {/each}
        </div>
    </div>
</div>
