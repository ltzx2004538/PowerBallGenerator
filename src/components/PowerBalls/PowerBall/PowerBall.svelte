<script lang="ts">
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let number: any;

  const handleClick = (id: number) => {
    dispatch("message", {
      id
    });
  };

  $: className = `${number.selected ? "powerball powerball__selected" : "powerball"}`

</script>

<button
  class={className}
  on:click={handleClick(number.id)}
>
  {number.value}
</button>

<style lang="scss">
  .selected {
    color: red;
  }
  .powerball {
    display: flex;
    align-items: center;
    justify-content: center;
    color: black;
    padding: 1rem;
    height: 50px;
    width: 50px;
    border: 1px solid black;
    border-radius: 50%;
    cursor: pointer;
    background-color: transparent;
    border: solid 1px black;
    font-size: 25px;

	&__selected{
		background-color: #3653a4;
		color: white
	}
  }
</style>
