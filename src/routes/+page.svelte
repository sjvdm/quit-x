<script>
  import CounterBox from '$lib/CounterBox.svelte';
  import { onMount } from "svelte";

  // Initialize counters array
  let counters = [{ count: 0, name: "X", lastIncrement: "Never" }];

  // Load the counters array from localStorage on mount
  onMount(() => {
    const savedCounters = localStorage.getItem("counters");
    if (savedCounters) {
      counters = JSON.parse(savedCounters);
    }
  });

  // Save the updated counters array to localStorage
  function saveCountersToLocalStorage() {
    localStorage.setItem("counters", JSON.stringify(counters));
  }

  // Increment a specific counter and save to localStorage
  function incrementCounter(index) {
    counters[index].count++;
    counters[index].lastIncrement = new Date().toLocaleString();
    saveCountersToLocalStorage();
  }

  // Reset a specific counter and save to localStorage
  function resetCounter(index) {
    counters[index].count = 0;
    counters[index].lastIncrement = "Never";
    saveCountersToLocalStorage();
  }

  // Save a custom name for the counter and save to localStorage
  function saveCounterName(index, newName) {
    counters[index].name = newName || "X";
    saveCountersToLocalStorage();
  }

  // Add a new counter box (up to 3 total) and save to localStorage
  function addCounter() {
    if (counters.length < 3) {
      counters = [...counters, { count: 0, name: "X", lastIncrement: "Never" }];
      saveCountersToLocalStorage();
    }
  }

  // Delete a counter box and save to localStorage
  function deleteCounter(index) {
    counters.splice(index, 1);
    counters = [...counters];
    saveCountersToLocalStorage();
  }
</script>

<style>
  :global(body) {
    background: url('/images/background.svg') no-repeat center center fixed;
    background-size: cover;
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
  }

  .container {
    width: 90%;
    max-width: 500px;
    padding: 20px;
  }

  .add-delete {
    font-size: 1em;
    color: #3182ce;
    cursor: pointer;
    text-align: center;
    margin: 20px 0;
  }
</style>

<div class="container">
  {#each counters as counter, index}
    <CounterBox
      {counter}
      {index}
      incrementCounter={incrementCounter}
      resetCounter={resetCounter}
      deleteCounter={deleteCounter}
      saveCounterName={saveCounterName}
    />
  {/each}

  {#if counters.length < 3}
    <button class="add-delete" on:click={addCounter}>+ Add Counter</button>
  {/if}
</div>