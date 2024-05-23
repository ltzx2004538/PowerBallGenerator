<script lang="ts">
  import PowerBall from "./PowerBall/PowerBall.svelte";
  import { getRandomInt } from "$utilities/utilities.ts";

  interface powerNumbers {
    id: number;
    value: number;
    selected: boolean;
  }

  const totalNumber = 35;
  const totalPowerBalls = 20;
  const maxNumber = 7;
  let selectedPowerBall: number | null;
  let selectedNumbers: Array<powerNumbers> = [];
  const result = (() => {
    const result = [];
    for (let i = 0; i < maxNumber; i++) {
      result.push({
        id: i
      });
    }
    return result;
  })();

  const getPowerBallObject = (total: number) => {
    const result = [];
    for (let i = 0; i < total; i++) {
      const powerBall = {
        id: i,
        value: i + 1,
        selected: false
      };
      result.push(powerBall);
    }
    return result;
  };

  let powerBalls = getPowerBallObject(totalPowerBalls);
  let powerNumbers = getPowerBallObject(totalNumber);
  selectedNumbers = [];

  const getIndexOfNumber = (id: number) => {
    const index = selectedNumbers.findIndex(sn => sn.id === id);
    return index;
  };

  const sortNumbers = () => {
    selectedNumbers = selectedNumbers.sort((a, b) => a.value - b.value);
  };

  const addNumber = (number: number) => {
    selectedNumbers.push(powerNumbers[number]);
    powerNumbers[number].selected = true;
  };

  const onClickNumbers = (event: any) => {
    const id = event.detail.id;
    const existNumberIndex = getIndexOfNumber(id);
    if (existNumberIndex >= 0) {
      selectedNumbers.splice(existNumberIndex, 1);
      powerNumbers[id].selected = false;
    } else if (selectedNumbers.length < 7) {
      addNumber(id);
    }
    sortNumbers();
  };

  const onClickPowerball = (event: any) => {
    const id = event.detail.id;
    powerBalls = getPowerBallObject(totalPowerBalls);
    selectedPowerBall = powerBalls[id].value;
    powerBalls[id].selected = true;
  };

  const handleClear = () => {
    powerBalls = getPowerBallObject(totalPowerBalls);
    powerNumbers = getPowerBallObject(totalNumber);
    selectedNumbers = [];
    selectedPowerBall = null;
  };

  const handleRandom = () => {
    handleClear();
    const runRandomTimes = maxNumber - selectedNumbers.length;
    const selectedSet = new Set(selectedNumbers.map(s => s.id));
    let randomSet = new Set<number>();
    for (let i = 0; i < runRandomTimes; i++) {
      const random = getRandomInt(0, 34);
      if (!selectedSet.has(random) && !randomSet.has(random)) {
        randomSet.add(random);
      } else {
        i -= 1;
      }
    }
    const randomPowerball = getRandomInt(0, 19);
    selectedPowerBall = powerBalls[randomPowerball].value;
    powerBalls[randomPowerball].selected = true;
    randomSet.forEach(randomNumber => {
      addNumber(randomNumber);
      sortNumbers();
    });
  };
</script>

<main>
  <div class="powerballs-container">
    <div class="powerballs-container__selected-numbers">
      {#each result as pickerNumber (pickerNumber.id)}
        <div class="picked-number-container">
          {#if selectedNumbers[pickerNumber.id]?.value}
            <span class="picked-number-container__number">
              {selectedNumbers[pickerNumber.id]?.value}</span
            >
          {/if}
        </div>
      {/each}
      <div class="picked-number-container">
        {#if selectedPowerBall}
          <span class="picked-number-container__number">
            {selectedPowerBall}</span
          >
        {/if}
      </div>
    </div>
    <div class="actions">
      <button
        class="actions__button action-button--random"
        on:click={handleRandom}
      >
        random
      </button>
      <button
        class="actions__button action-button--clear"
        on:click={handleClear}
      >
        clear
      </button>
    </div>

    <div class="label">select your number</div>
    <div class="numbers">
      {#each powerNumbers as number (number.id)}
        <PowerBall on:message={onClickNumbers} {number} />
      {/each}
    </div>
    <div class="label">select your power ball</div>
    <div class="numbers powerballs">
      {#each powerBalls as powerball (powerball.id)}
        <PowerBall on:message={onClickPowerball} number={powerball} />
      {/each}
    </div>
    <div class="footer">
      <span class="footer__label"> version 1.1</span>
      <span class="footer__label">powered by Yurun</span>
    </div>
  
  </div>
 
</main>

<style lang="scss">
  @import './PowerBalls.scss';
</style>
