<script>
    import { createEventDispatcher } from "svelte";
    export let noSolution = false;
    const dispatch = createEventDispatcher();
    let files = undefined;

    const logFile = (event) => {
        dispatch("setGamePlayOptions", {
            file: JSON.parse(event.target.result),
        });
    };

    const sendOptions = async () => {
        if (files) {
            let reader = new FileReader();
            reader.onload = logFile;
            reader.readAsText(files[0]);
        } else {
            dispatch("setGamePlayOptions", {
                file: undefined,
            });
        }
    };
</script>

<div class="w-full h-full flex justify-center items-center">
    <div
        class="border-2 rounded-lg border-gray-200 w-1/4 h-2/6 bg-gray-100 shadow-xl flex flex-col divide-y divide-gray-200"
    >
        <div
            class="w-full h-2/6 flex justify-center items-center text-3xl text-gray-600 font-bold text-center"
        >
            Wgraj plik z sudoku
        </div>

        <div
            class="w-full h-2/6 flex flex-row text-lg justify-start items-center text-gray-600 font-semibold px-6"
        >
            <div class="w-1/2 flex justify-start items-end">
                Własny plik JSON:
            </div>
            <div class="w-1/2 flex justify-center items-end">
                <input
                    bind:files
                    type="file"
                    id="actual-btn"
                    accept=".json"
                    hidden
                />
                <label
                    for="actual-btn"
                    class="text-base border border-gray-300 cursor-pointer p-1 rounded-md bg-white  w-auto  text-center px-4 truncate h-7 flex items-center justify-center"
                    >{#if files}
                        {files[0].name}
                    {:else}
                        ---
                    {/if}</label
                >
            </div>
        </div>

        <div
            class="w-full h-2/6 flex  text-lg justify-center items-center text-gray-600 font-semibold px-6 flex-col"
        >
            <button
                on:click={sendOptions}
                class="w-2/5 shadow-md bg-blue-300 py-1 flex justify-center items-center rounded-lg hover:bg-blue-200 hover:text-white border-blue-500 focus:border-2 transition-transform text-white"
            >
                Zagraj
            </button>
            {#if noSolution}
                <h1 class="text-lg text-red-400">Brak rozwiązania!</h1>
            {/if}
        </div>
    </div>
</div>
