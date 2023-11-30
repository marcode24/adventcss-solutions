<script>
  import { onMount } from 'svelte';

  let minutes = 0;
  let seconds = 15;
  let counter = minutes * 60 + seconds;
  let counterInitial = counter;
  let counterTemp = 0;
  let minutesInitial = minutes;
  let secondsInitial = seconds;
  let startCounter;
  let stoped = true;

  let progressStyle = '';
  let btnStartDisplay = 'block';
  let btnStopDisplay = 'none';
  let gearDisplay = 'block';
  let checkDisplay = 'none';
  let timeDisplay = 'block';
  let btnStopText = 'Stop';

  let minutesInputValue = '';
  let secondsInputValue = '';

  let timerInputDisplay = 'none';

  onMount(() => {
    startCounter = setInterval(() => {
      if (!stoped) {
        counter--;
        minutes = Math.floor(counter / 60);
        console.log(minutes);
        seconds = counter % 60;
        updateTime();
        if (counter === 0) finish();
      }
    }, 1000);
  });

  function updateTime() {
    const degress = (counter * 360) / counterInitial;

    if (degress === 0) {
      progressStyle = `conic-gradient(#00aa51 360deg, #00aa51 0deg)`;
    } else {
      progressStyle = `conic-gradient(#9d0000 ${degress}deg, #010100 0deg)`;
    }

    if (minutes < 10) minutes = "0" + minutes;
    if (seconds < 10) seconds = "0" + seconds;
  }

  function finish() {
    stoped = true;
    btnStartDisplay = "block";
    btnStopDisplay = "none";
    gearDisplay = "block";
    timeDisplay = `${minutesInitial}:${secondsInitial}`;
    counter = Number(minutesInitial) * 60 + Number(secondsInitial);
    counterInitial = counter;
  }

  function handleGearClick() {
    gearDisplay = "none";
    checkDisplay = "block";
    timerInputDisplay = "flex";
    timeDisplay = "none";
    btnStartDisplay = "none";
    btnStopDisplay = "none";
  }

  function handleCheckClick() {
    btnStartDisplay = "block";
    timerInputDisplay = "none";
    gearDisplay = "block";
    checkDisplay = "none";
    timeDisplay = "block";

    minutes = Number(minutesInputValue);
    seconds = Number(secondsInputValue);
    minutesInitial = minutes;
    secondsInitial = seconds;
    counter = minutes * 60 + seconds;
    counterInitial = counter;

    updateTime();
  }

  function handleStartClick() {
    if (counterInitial > 0) {
      stoped = false;
      btnStartDisplay = "none";
      btnStopDisplay = "block";
      gearDisplay = "none";
    }
  }

  function handleStopClick() {
    if (counterTemp === 0 && !stoped) {
      btnStopText = "Continue";
      stoped = true;
    } else {
      stoped = false;
      btnStopText = "Stop";
    }
  }

  function validateNumber(value) {
    const numValue = Math.ceil(Number(value));
    if (numValue > 60) {
      return 60;
    } else if (numValue < 0) {
      return 0;
    }
    return numValue;
  }

  function validateWhenFocusOut(value) {
    const numValue = Math.ceil(Number(value));
    if (numValue >= 0 && numValue < 10) {
      return "0" + numValue;
    }
    return numValue;
  }

  function handleMinutesInputKeyUp(event) {
    validateNumber(event);
  }

  function handleMinutesInputFocusOut(event) {
    validateWhenFocusOut(event);
  }

  function handleSecondsInputKeyUp(event) {
    validateNumber(event);
  }

  function handleSecondsInputFocusOut(event) {
    validateWhenFocusOut(event);
  }
</script>

<style lang="scss">
  @import 'src/styles/challenge01';
</style>

<section class="challenge">
  <div class="progress">
    <div id="timer" class="timer">
      <div class="timer-container">
        <span id="time" class="time">{minutes }:{seconds}</span>
        <div class="timer-input" style="display: {timerInputDisplay}">
          <input bind:value={minutesInputValue} type="number" min="0" max="60">
          <span>:</span>
          <input bind:value={secondsInputValue} type="number" min="0" max="60">
        </div>
      </div>
      <div class="buttons">
        <button on:click={handleStartClick} style="display: {btnStartDisplay}" class="start">Start</button>
        <button on:click={handleStopClick} style="display: {btnStopDisplay}" class="stop">{btnStopText}</button>
        <div on:click={handleGearClick} style="display: {gearDisplay}" class="icon gear"></div>
        <div on:click={handleCheckClick} style="display: {checkDisplay}" class="icon check"></div>
      </div>
    </div>
  </div>
</section>
