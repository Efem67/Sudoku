<script lang="ts">
    import GameBoard from "../components/GameBoard.svelte";
    import GameOptionsMenu from "../components/GameOptionsMenu.svelte";
    import Header from "../components/Header.svelte";

    let gameStarted = false;
    let gameBoardArr = [];
    let X = 9;
    let solvedSudoku;
    let amountArray = [];
    let doneSudoku = false;
    let noSolution = false;

    function solveSudoku(grid, row, col) {
        if (row == X - 1 && col == X) return true;

        if (col == X) {
            row++;
            col = 0;
        }

        if (grid[row][col] != 0) return solveSudoku(grid, row, col + 1);

        for (let num = 1; num < 10; num++) {
            if (isSafe(grid, row, col, num)) {
                grid[row][col] = num;

                if (solveSudoku(grid, row, col + 1)) return true;
            }
            grid[row][col] = 0;
        }
        return false;
    }

    function isSafe(grid, row, col, num) {
        for (let x = 0; x <= 8; x++) if (grid[row][x] == num) return false;

        for (let x = 0; x <= 8; x++) if (grid[x][col] == num) return false;

        let startRow = row - (row % 3),
            startCol = col - (col % 3);

        for (let i = 0; i < 3; i++)
            for (let j = 0; j < 3; j++)
                if (grid[i + startRow][j + startCol] == num) return false;

        return true;
    }

    const checkWin = () => {
        doneSudoku = true;
        gameBoardArr.forEach((row, i) => {
            row.forEach((value, j) => {
                if (value.content.value != solvedSudoku[i][j])
                    doneSudoku = false;
            });
        });
    };

    const checkNumbersAmount = () => {
        amountArray = [];
        for (let i = 1; i <= 9; i++) {
            amountArray = [...amountArray, { value: i, amountLeft: 0 }];
        }
        console.log(amountArray);
        gameBoardArr.forEach((row, i) => {
            row.forEach((value, j) => {
                if (value.solid) return;
                if (value.content.correct) return;
                amountArray.map((el) => {
                    if (el.value == solvedSudoku[i][j]) {
                        return {
                            value: el.value,
                            amountLeft: (el.amountLeft += 1),
                        };
                    } else return el;
                });
            });
        });
        checkWin();
    };

    const setOptions = (options) => {
        if (options.file.gameState) {
            console.log(options.file);
            solvedSudoku = options.file.solution;
            gameBoardArr = options.file.arr;
            gameStarted = true;
            checkNumbersAmount();
            return;
        }
        if (options.file.arr) {
            solvedSudoku = JSON.parse(JSON.stringify(options.file.arr));
            if (!solveSudoku(solvedSudoku, 0, 0)) {
                console.log("Brak rozwiązania!");
                noSolution = true;
                return;
            }

            gameStarted = true;
            let arr = [];
            options.file.arr.forEach((row, i) => {
                let rowArr = [];
                row.forEach((value, j) => {
                    let obj = {
                        solid: value != 0 ? true : false,
                        content: {
                            hint: [],
                            value,
                            correct: false,
                        },
                        currentSelected: i == 0 && j == 0 ? true : false,
                    };
                    rowArr.push(obj);
                });
                arr.push(rowArr);
            });

            gameBoardArr = arr;
            checkNumbersAmount();
        }
    };

    const changeCurrSelected = (detail) => {
        gameBoardArr[detail.last.i][detail.last.j].currentSelected = false;
        gameBoardArr[detail.new.i][detail.new.j].currentSelected = true;
    };
    const fillField = (detail) => {
        gameBoardArr.forEach((row, i) => {
            row.forEach((value, j) => {
                if (value.currentSelected) {
                    if (value.solid) return;
                    gameBoardArr[i][j].content.value = 0;

                    if (detail.name == "hint") {
                        gameBoardArr[i][j].content.correct = false;
                        gameBoardArr[i][j].content[detail.name] = [
                            ...gameBoardArr[i][j].content[detail.name],
                            detail.value,
                        ];
                    }

                    if (detail.name == "value") {
                        gameBoardArr[i][j].content.hint = [];
                        if (solvedSudoku[i][j] == detail.value)
                            gameBoardArr[i][j].content.correct = true;
                        else gameBoardArr[i][j].content.correct = false;
                        gameBoardArr[i][j].content[detail.name] = detail.value;
                    }
                }
            });
        });
        checkNumbersAmount();
    };
    const removeFromField = () => {
        gameBoardArr.forEach((row, i) => {
            row.forEach((value, j) => {
                if (value.currentSelected) {
                    if (value.solid) return;
                    gameBoardArr[i][j].content.value = 0;
                    gameBoardArr[i][j].content.hint = [];
                    gameBoardArr[i][j].content.correct = false;
                }
            });
        });
        checkNumbersAmount();
    };
    const showSolved = () => {
        gameBoardArr.forEach((row, i) => {
            row.forEach((value, j) => {
                if (value.solid) return;
                gameBoardArr[i][j].content.value = solvedSudoku[i][j];
                gameBoardArr[i][j].content.hint = [];
                gameBoardArr[i][j].content.correct = true;
            });
        });
        checkNumbersAmount();
    };

    const removeOnlyHints = () => {
        gameBoardArr.forEach((row, i) => {
            row.forEach((value, j) => {
                if (value.solid) return;
                if (value.content.value) return;
                gameBoardArr[i][j].content.hint = [];
                gameBoardArr[i][j].content.correct = false;
            });
        });
        checkNumbersAmount();
    };

    const download = () => {
        let content = {
            gameState: true,
            solution: solvedSudoku,
            arr: gameBoardArr,
        };
        const a = document.createElement("a");
        const file = new Blob([JSON.stringify(content)], {
            type: "text/plain",
        });
        a.href = URL.createObjectURL(file);
        a.download = "saved.json";
        a.click();
    };
</script>

<div
    class="w-full h-screen bg-gray-white flex flex-col items-center overflow-hidden"
>
    {#if !gameStarted}
        <GameOptionsMenu
            {noSolution}
            on:setGamePlayOptions={({ detail }) => setOptions(detail)}
        />
    {:else}
        <Header
            on:showSolved={() => showSolved()}
            on:saveGameState={() => download()}
        />
        <div class="w-full flex flex-col h-full justify-center items-center">
            <GameBoard
                {doneSudoku}
                {amountArray}
                array={gameBoardArr}
                on:removeFromField={() => removeFromField()}
                on:removeHints={() => removeOnlyHints()}
                on:arrowsMoveEvent={({ detail }) => changeCurrSelected(detail)}
                on:fillField={({ detail }) => fillField(detail)}
            />
        </div>
    {/if}
</div>
