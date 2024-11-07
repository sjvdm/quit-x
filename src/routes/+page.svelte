<script>
  import CounterBox from '$lib/CounterBox.svelte';
  import { onMount } from "svelte";

  // Initialize counters array
  let counters = [
    { count: 10, name: "Facebook", lastIncrement: "Never" },
    { count: 3, name: "Anger", lastIncrement: "Never" }
  ];

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

  // FAQ functionality
  let openFAQ = null;

  function toggleFAQ(index) {
    openFAQ = openFAQ === index ? null : index;
  }
</script>

<style>

   @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap');

  :global(body) {
    background: url('/images/background.svg') no-repeat center center fixed;
    background-size: cover;
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin: 0;
    min-height: 100vh;
  }

  .logo-container {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 20px;
  }

  .logo {
    width: 100px;
    height: auto;
  }

  .container {
    width: 90%;
    max-width: 500px;
    min-width: 100vh;
    padding: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .add-delete {
    font-size: 1em;
    color: #3182ce;
    cursor: pointer;
    text-align: center;
    margin: 20px 0;
  }

  /* FAQ Section */
  .faq-container {
    width: 90%;
    max-width: 500px;
    margin-top: 40px;
    text-align: center;
  }

  .faq-item {
    border: 1px solid #ccc;
    border-radius: 5px;
    margin-bottom: 10px;
    padding: 10px;
    cursor: pointer;
  }

  .faq-question {
    font-weight: bold;
  }

  .faq-answer {
    padding: 10px;
    display: none;
  }

  .faq-item.open .faq-answer {
    display: block;
  }

    .h1-main {
        font-family: 'Montserrat', sans-serif;
        color: #3C67B1;
        text-align: center;
        margin-top: 0px;
    }

</style>

<!-- Logo Section -->
<div class="logo-container" style="margin-top: 20px;">
  <!-- Placeholder SVG logo -->
  <svg width="100" height="100" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" fill="none" stroke="#3C67B1" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <path d="M12 2C10.5 2 8 7 8 11c0 4 1.5 6.5 4 9s4-2.5 4-6c0-4-2-9-4-9zm0 0C8 6 3 11 3 16c0 1.5 1.5 3 3 3 3.5 0 4-3 7-3 1 0 3 1 4 2" />
</svg>\
</div>
<h1 class="h1-main">Keep Yourself Accountable</h1>

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

<!-- Centered FAQ Section at the bottom -->
<div class="faq-container">
  <h2>Frequently Asked Questions</h2>

  {#each ['What is this app for?', 'How do I add a counter?', 'How do I reset a counter?', 'Is my data stored?'] as question, index}
    <div class="faq-item {openFAQ === index ? 'open' : ''}" on:click={() => toggleFAQ(index)}>
      <div class="faq-question">{question}</div>
      <div class="faq-answer">
        {#if index === 0}
          <p>This app helps you track counts for various activities or goals.</p>
        {:else if index === 1}
          <p>To add a counter, simply click the "Add Counter" button.</p>
        {:else if index === 2}
          <p>You can reset a counter by clicking the "Reset" button next to each counter.</p>
        {:else if index === 3}
          <p>No information is stored anywhere, except locally on your browser.</p>
        {/if}
      </div>
    </div>
  {/each}
</div>