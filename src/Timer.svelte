<script>
  import { createEventDispatcher } from 'svelte';
  import ProgressBar from './ProgressBar.svelte';

  const totalSeconds = 20;
  let secondsLeft = totalSeconds;
  $: progress = ((totalSeconds - secondsLeft) / totalSeconds) * 100;
  let isRunning = false;

  const dispatch = createEventDispatcher();
  function startTimer() {
    isRunning = true;
    const timer = setInterval(() => {
      secondsLeft -= 1;
      if (secondsLeft == 0) {
        clearInterval(timer);
        isRunning = false;
        secondsLeft = totalSeconds;
        dispatch('end');
      }
    }, 1000);
  }
</script>

<style>
  h2 {
    margin: 0;
  }
  .start {
    background-color: rgb(154, 73, 73);
    width: 100%;
    margin: 10px 0;
  }

  .start[disabled] {
    background-color: rgb(194, 194, 194);
    cursor: not-allowed;
  }
</style>

<div bp="grid">
  <h2
    bp="offset-5@md 4@md 12@sm"
    src="handwashing.gif"
    alt="How to wash your hands">
    Seconds Left: {secondsLeft}
  </h2>
</div>

<ProgressBar {progress} />
<div bp="grid">
  <button
    disabled={isRunning}
    on:click={startTimer}
    bp="offset-5@md 4@md 12@sm"
    class="start">
    Start
  </button>
</div>
