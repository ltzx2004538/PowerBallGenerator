<script lang="ts">
  import PowerBall from "$components/PowerBall.svelte";

  interface powerball {
    id: number;
    value: number;
    selected: boolean;
  }

  const totalNumber = 35;
  const totalPowerBalls = 20;
  const maxNumber = 7;
  let selectedPowerBall: number | null;
  let selectedNumbers: Array<powerball> = [];

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

  $: selections = `${
    selectedNumbers.length
      ? selectedNumbers
          .map(s => s.value)
          .sort((a, b) => a - b)
          .join(" ")
      : ""
  } ${selectedPowerBall ?? " "}`;

  const getIndexOfNumber = (id: number) => {
    const index = selectedNumbers.findIndex(sn => sn.id === id);
    return index;
  };

  const onClickNumbers = (event: any) => {
    const id = event.detail.id;
    if (selectedNumbers.length >= 7) {
      return;
    }
    powerNumbers[id].selected = !powerNumbers[id].selected;
    const existNumberIndex = getIndexOfNumber(id);
    if (existNumberIndex >= 0) {
      selectedNumbers.splice(existNumberIndex, 1);
    } else {
      selectedNumbers.push(powerNumbers[id]);
    }
    selectedNumbers = selectedNumbers.sort((a, b) => a - b);
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
</script>

<main>
  <div class="powerballs-container">
    <div>{selections}</div>
    <button on:click={handleClear}> clear </button>
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
  </div>
</main>

<style lang="scss">
  .powerballs-container {
    margin: auto;
    display: flex;
    flex-direction: column;
    width: 800px;
    gap: 40px;
  }
  .numbers {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    width: 800px;
  }
  .label {
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>
