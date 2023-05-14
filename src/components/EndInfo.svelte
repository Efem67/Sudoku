<script>
    export let gameResult;
    export let hiddenWord;
    export let startNewGame;
    export let stillOpenGameInfo;
</script>

<div
    class="w-full h-full flex justify-center items-center absolute bg-gray-200 bg-opacity-60"
>
    <div
        class="border-2 rounded-lg border-gray-200 w-1/3 h-2/5 bg-gray-100 shadow-xl flex flex-col divide-y divide-gray-200 p-4"
    >
        <div
            class="w-full h-2/5 flex justify-center items-center text-3xl text-gray-600 font-bold text-center"
        >
            Wynik rozgrywki
        </div>
        <div
            class="w-full h-1/5 flex flex-row text-lg justify-start items-center text-gray-600 font-semibold px-6"
        >
            <div class="w-1/2 flex justify-start items-end">Wynik:</div>
            <div class="w-1/2 flex justify-center items-end">
                {#if gameResult == "Win"}
                    <span>Wygrana</span>
                    <img
                        src="src/assets/winner.png"
                        alt="settings"
                        class="w-8 h-8 ml-2"
                    />
                {:else}
                    <span>Przegrana</span>
                    <img
                        src="src/assets/sad.png"
                        alt="settings"
                        class="w-7 h-7 ml-2"
                    />
                {/if}
            </div>
        </div>
        <div
            class="w-full h-1/5 flex flex-row text-lg justify-start items-center text-gray-600 font-semibold px-6"
        >
            <div class="w-1/2 flex justify-start items-end">Ukryty wyraz:</div>
            <div class="w-1/2 flex justify-center items-end">
                {hiddenWord.toUpperCase()}
            </div>
        </div>

        <div
            class="w-full h-2/5 flex flex-row text-lg justify-center items-center text-gray-600 font-semibold px-6"
        >
            <button
                on:click={() => {
                    startNewGame = false;
                    stillOpenGameInfo = false;
                }}
                class="whitespace-nowrap w-auto px-4 shadow-md bg-green-300 py-1 flex justify-center items-center rounded-lg hover:bg-green-200 hover:text-gray-500 border-green-500 focus:border-2"
            >
                Zagraj ponownie
            </button>
        </div>
    </div>
</div>
