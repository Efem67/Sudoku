<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
</script>

<div
    class="w-full h-20  border-gray-300 border-b flex items-center shadow-lg bg-white"
>
    <div class="w-2/5" />
    <div class="w-1/5 text-center  flex flex-row items-center justify-center">
        <img
            src="src/assets/sudoku.png"
            alt="settings"
            class="w-14 h-14 mr-2"
        />
        <h1 class="text-4xl font-sans font-bold font">Sudoku</h1>
    </div>

    <div class="w-2/5 flex flex-row items-center justify-end px-6">
        <button on:click={() => dispatch("showSolved")}>
            <img
                src="src/assets/problem-solve.png"
                alt="settings"
                class="w-9 h-9 cursor-pointer"
            />
        </button>
        <button class="ml-4" on:click={() => dispatch("saveGameState")}>
            <img
                src="src/assets/diskette.png"
                alt="settings"
                class="w-9 h-9 cursor-pointer"
            />
        </button>
    </div>
</div>

<style>
    .font {
        color: white;
        font-size: 60px;
        -webkit-text-stroke-width: 4px;
        -webkit-text-stroke-color: rgb(70, 161, 236);
    }
</style>
