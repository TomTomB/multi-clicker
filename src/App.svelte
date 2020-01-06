<script>
  const layoutStart = "LAYOUT_START";
  const layoutIngame = "LAYOUT_INGAME";
  const layoutFinish = "LAYOUT_FINISH";
  const defaultTime = 5;

  let gameLayout = layoutStart;
  let time = defaultTime;
  let timer = 0;
  let score = 0;
  $: apm = (score / (defaultTime - time)).toFixed(2);

  function startGame() {
    gameLayout = layoutIngame;

    timer = setInterval(() => {
      time--;
      if (time === 0) {
        gameLayout = layoutFinish;
        clearInterval(timer);
      }
    }, 1000);
  }

  function abordGame() {
    resetGame();
  }

  function addToScore() {
    score = score + 1;
  }

  function resetGame() {
    clearInterval(timer);
    gameLayout = layoutStart;
    time = defaultTime;
    timer = 0;
    score = 0;
  }
</script>

<style>
  h1 {
    text-align: center;
    background: #fafafa;
    margin: 0;
    padding: 1rem;
  }
  .actions {
    display: flex;
    justify-content: center;
    margin-top: 4rem;
    position: relative;
  }

  button.btn-primary {
    width: 200px;
    height: 200px;
    border-radius: 100vw;
    font-size: 2.5rem;
    text-transform: uppercase;
  }
  button.opacity {
    opacity: 0.9;
  }

  .stats {
    display: flex;
    margin-top: 4rem;
  }

  .stats .row {
    flex: 1;
    text-align: center;
  }

  .finish-text {
    text-align: center;
    margin-top: 3rem;
  }

  .score-effect {
    font-size: 6rem;
    position: absolute;
    top: 35%;
    left: 50%;
    margin: 0;
    line-height: 1;
    font-variant-numeric: tabular-nums;
    transform: translate(-50%, -50%);
    pointer-events: none;
    animation: pulse 300ms ease-in-out alternate infinite;
    z-index: -1;
  }

  @keyframes pulse {
    from {
      transform: translate(-50%, -50%) scale(2);
      opacity: 1;
    }
    to {
      transform: translate(-50%, -50%) scale(3.5);
      opacity: 0.25;
    }
  }
</style>

<main>
  {#if gameLayout === layoutStart}
    <h1>Multi Clicker</h1>
    <div class="actions">
      <button class="btn btn-primary" on:click={startGame}>Start</button>
    </div>
  {/if}

  {#if gameLayout === layoutIngame}
    <h1>Click dat Shit!</h1>
    <div class="actions">
      <button class="btn btn-primary opacity" on:click={addToScore}>
        Click
      </button>
      <p class="score-effect">{score}</p>
    </div>
    <div class="stats">
      <div class="row">
        <h3>Clicks</h3>
        <p>{score}</p>
      </div>
      <div class="row">
        <h3>APM</h3>
        <p>{apm}</p>
      </div>
      <div class="row">
        <h3>Zeit</h3>
        <p>{time}</p>
      </div>
    </div>
    <div class="actions">
      <button class="btn btn-small" on:click={abordGame}>Abbrechen</button>
    </div>
  {/if}

  {#if gameLayout === layoutFinish}
    <h1>Du bischt der King!</h1>
    <p class="finish-text">Dein Score sieht wie folgt aus</p>
    <div class="stats">
      <div class="row">
        <h3>Clicks</h3>
        <p>{score}</p>
      </div>
      <div class="row">
        <h3>APM</h3>
        <p>{apm}</p>
      </div>
      <div class="row">
        <h3>Zeit</h3>
        <p>{defaultTime}</p>
      </div>
    </div>
    <div class="actions">
      <button class="btn btn-small" on:click={abordGame}>
        Zu Start zurück
      </button>
    </div>
  {/if}

</main>
