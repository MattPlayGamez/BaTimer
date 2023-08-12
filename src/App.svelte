<script>
  let targetTime = 0;
  let remainingTime = 0;
  let percentComplete = 0;
  let interval;
  let value;
  let timeDiff = 0;

  const startTimer = () => {
    if (interval) clearInterval(interval);

    interval = setInterval(() => {
      const currentTime = new Date().getTime();
      timeDiff = targetTime - currentTime;

      if (timeDiff > 0) {
        remainingTime = timeDiff;
        percentComplete =
          ((targetTime - currentTime) /
            (targetTime - new Date().setSeconds(0))) *
          100;
      } else {
        clearInterval(interval);
        remainingTime = 0;
        alert(`${value} minuten is over`)
        // percentComplete = 100;
      }
    }, 500);
  };

  const formatTime = (milliseconds) => {
    const minutes = Math.floor(milliseconds / 60000);
    const seconds = Math.floor((milliseconds % 60000) / 1000);
    return `${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;
  };

  const setTimer = (minutes) => {
    targetTime = new Date().getTime() + minutes * 60000;
    startTimer();
  };
</script>

<main>
  <div class="timer">
    <input class="btn" type="number" placeholder="Enter minutes" bind:value />
    {#if timeDiff == 0 || timeDiff == null}
    <button on:click={() => setTimer(value)}>Set Timer</button>
    {:else}
    <button on:click={() => setTimer(value)}>Reset Timer</button>

    {/if}
    <div class="progress">
      <div class="battery" style="width: {percentComplete}%;" />
    </div>
    <p class="time">{formatTime(remainingTime)}</p>
  </div>
</main>

<style>
  .timer {
    text-align: center;
    margin: 2rem auto;
  }

  .progress {
    width: 100%;
    height: 20px;
    background-color: #f0f0f0;
    border-radius: 10px;
    overflow: hidden;
  }

  .battery {
    height: 100%;
    background-color: #4caf50;
    transition: width 0.5s;
  }

  .time {
    font-size: 1.5rem;
    margin-top: 1rem;
  }
  main {
    transform: scale(2);
  }
</style>
