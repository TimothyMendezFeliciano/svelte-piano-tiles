<script>
    let rows = []
    let score = 0
    let gameover = false

    function generateRow() {
        let row = new Array(4).fill("white");
        let position = Math.trunc(Math.random() * 4);
        row[position] = "black";
        return row
    }

    function fillRows() {
        for (let i = 0; i < 4; i++) {
            rows.push(generateRow())
        }
    }

    function tapped(i, j) {
        if (i !== rows.length - 1 || rows[i][j] === "white") {
            gameover = true
            rows[i][j] = "red"
        } else {
            rows.splice(i);
            rows = [generateRow(), ...rows]
            score++;
        }
    }

    function restart() {
        score = 0;
        gameover = false
        rows = []
        fillRows();
    }

    fillRows()
</script>

<style global>
    .app {
        position: fixed;
        top: 0px;
        left: 50%;
        transform: translateX(-50%);
        width: 100%;
        height: 100%;
        max-width: 400px;
    }

    .app .header {
        position: sticky;
        top: 0px;
        left: 0px;
        width: 100%;
        height: 50px;
        background: #0badea;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0px 20px;
        color: #fff;
    }

    .app .game {
        width: 100%;
        height: calc(100% - 50px);
        background: #fff;
    }

    .app .game .row {
        display: flex;
        width: 100%;
        height: calc(100% / 4);
    }

    .app .game .row .box {
        flex: 1;
        border: 1px solid #555;
        cursor: pointer;
    }

    .app .game .row .box.black {
        background: #111;
    }

    .app .game .row .box.red {
        animation: blinkRed 500ms ease-in-out infinite;
    }

    @keyframes blinkRed {
        0%, 100% {
            background: #fff;
        }
        50% {
            background: tomato;
        }
    }

    .result {
        position: absolute;
        top: 0px;
        left: 0px;
        width: 100%;
        height: 100%;
        background: rgba(0,0,0,0.8);
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        gap: 10px;
    }
    .result h2 {
        font-size: 40px;
        color: #fff;
    }
    .result p {
        font-size: 20px;
        margin-bottom: 10px;
        color: #eee;
    }
    .result buton {
        padding: 10px 20px;
        cursor: pointer;
        font-size: 16px;
        border: 2px solid #fff;
        color: #fff;
        outline: none;
        font-weight: 600;
        background: transparent;
    }
</style>

<main class="app">
    <div class="header">
        <h4>Piano Tiles</h4>
        <p>score: {score}</p>
    </div>
    {#if gameover}
        <div class="result">
            <h2>Game Over</h2>
            <p>Score: {score}</p>
            <button on:click={restart}>Try Again</button>
        </div>
    {/if}
    <div class="game">
        {#each rows as row,i}
            <div class="row">
                {#each row as box,j}
                    <div
                            class={"box "+box}
                            on:click={()=>tapped(i,j)}
                    ></div>
                {/each}
            </div>
        {/each}
    </div>
</main>