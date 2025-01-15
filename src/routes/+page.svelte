<script>
  import CounterBox from '$lib/CounterBox.svelte';
  import { onMount } from "svelte";

  // Initialize counters array
  let counters = [
    { count: 10, name: "Facebook Free", lastIncrement: "Never" },
    { count: 3, name: "Anger Free", lastIncrement: "Never" },
    { count: 20, name: "Act of kindness", lastIncrement: "Never" },
  ];

  // [ADDED] Track if the tooltip should display
  let showTooltip = false;

  // Load the counters array from localStorage on mount
  onMount(() => {
    const savedCounters = localStorage.getItem("counters");
    if (savedCounters) {
      counters = JSON.parse(savedCounters);
    }
    // [ADDED] Check if the tooltip has been seen before
    const tooltipSeen = localStorage.getItem("tooltipSeen");
    showTooltip = !tooltipSeen; // Show tooltip if it's not seen before
  });

  // [ADDED] Mark tooltip as seen
  function markTooltipSeen() {
    showTooltip = false;
    localStorage.setItem("tooltipSeen", "true");
  }

  // Save the updated counters array to localStorage
  function saveCountersToLocalStorage() {
    localStorage.setItem("counters", JSON.stringify(counters));
  }

  // Increment a specific counter and save to localStorage
  function incrementCounter(index) {
    counters[index].count++;
    counters[index].lastIncrement = new Date().toLocaleString();
    saveCountersToLocalStorage();
    // [ADDED] Mark tooltip as seen after the first interaction
    if (showTooltip) {
      markTooltipSeen();
    }

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

  :global(html, body) {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    width: 100%;
    overflow-x: hidden; /* [ADDED] Prevent horizontal scrolling */
  }


  :global(body) {
    background: url('/images/background.svg') repeat center center fixed;
    background-size: cover;
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin: 0;
    min-height: 100vh;

    /* [UPDATED] Ensure proper centering for mobile devices */
    padding: 0 10px;
    box-sizing: border-box;

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
    width: 100%; /* [UPDATED] Adjust width for smaller screens */
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

  /* [ADDED] Responsive adjustments for smaller screens */
  @media (max-width: 600px) {
    .container {
      max-width: 100%; /* Use full width */
      padding: 10px;   /* Reduce padding */
    }

    .logo {
      width: 80px; /* Smaller logo for mobile */
    }

    .premium-button {
      font-size: 0.9rem; /* Smaller button text */
      padding: 8px 15px; /* Reduced padding */
    }

    .add-delete {
      font-size: 0.9em; /* Adjust font size */
      padding: 10px; /* [ADDED] Touch-friendly padding */
    }
  }

  /* FAQ Section */
  .faq-container {
    width: 100%;
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
        font-size: 1.5em; /* [UPDATED] Adjust font size for mobile */
    }

    .h3-main {
        font-family: 'Montserrat', sans-serif;
        color: #3C67B1;
        text-align: center;
        margin-top: 0px;
        font-size: 1em; /* [UPDATED] Adjust font size for mobile */
    }

.premium-button {
    background-color: #007BFF; /* Adjust to match the site's primary color */
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 1rem;
    transition: background-color 0.3s ease;
  }

  .premium-button:hover {
    background-color: #0056b3; /* Slightly darker shade for hover effect */
  }

</style>

<!-- Logo Section -->
<div class="logo-container" style="margin-top: 20px;">
  <!-- Placeholder SVG logo -->
  <svg width="100" height="100" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" fill="none" stroke="#3C67B1" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <path d="M12 2C10.5 2 8 7 8 11c0 4 1.5 6.5 4 9s4-2.5 4-6c0-4-2-9-4-9zm0 0C8 6 3 11 3 16c0 1.5 1.5 3 3 3 3.5 0 4-3 7-3 1 0 3 1 4 2" />
</svg>
</div>
<h1 class="h1-main">Keep Yourself Accountable</h1>
<h1 class="h3-main">A simple, daily, accountability tracker</h1>

<div class="container">
  {#each counters as counter, index}
    <CounterBox
      {counter}
      {index}
      incrementCounter={incrementCounter}
      resetCounter={resetCounter}
      deleteCounter={deleteCounter}
      saveCounterName={saveCounterName}
      showTooltip={showTooltip && index === 0}
    />
  {/each}

  {#if counters.length < 3}
    <button class="add-delete" on:click={addCounter}>+ Add Counter</button>
  {/if}
</div>


<div style="text-align: center; margin-top: 20px;">
  <button class="premium-button" on:click={() => window.location.href='https://forms.gle/ThbEU48ebZqP1CJT7'}>
    Go Premium
  </button>
</div>

<!-- Centered FAQ Section at the bottom -->
<div class="faq-container">
  <h2>Frequently Asked Questions</h2>

  {#each ['What is this app for?', 'How do I add a counter?', 'How do I reset a counter?', 'Is my data stored?','Why the ads?'] as question, index}
    <div class="faq-item {openFAQ === index ? 'open' : ''}" on:click={() => toggleFAQ(index)}>
      <div class="faq-question">{question}</div>
      <div class="faq-answer">
        {#if index === 0}
          <p>This app helps you track counts for various activities or goals. Want to quit Facebook? Keep a counter - daily. Just click on the "days" counter!</p>
        {:else if index === 1}
          <p>To add a counter, simply click the "Add Counter" button. You can call the counter anything you want.</p>
        {:else if index === 2}
          <p>You can reset a counter by clicking the "Reset" button next to each counter.</p>
        {:else if index === 3}
          <p>No information is stored anywhere, except locally on your browser. You can verify by checking the code here: <a href="https://github.com/sjvdm/quit-x" target="_blank">Github repo</a></p>
        {:else if index === 4}
          <p>I have tried making the ads as non-intrusive as possible! Income is used to pay for hosting etc. The code is available here: <a href="https://github.com/sjvdm/quit-x" target="_blank">Github repo</a></p>
        {/if}
      </div>
    </div>
  {/each}
</div>

