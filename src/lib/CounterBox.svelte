<script>
  export let counter = { count: 0, name: "X", lastIncrement: "Never" };
  export let index;
  
  // Functions passed in as props
  export let incrementCounter;
  export let resetCounter;
  export let deleteCounter;
  export let saveCounterName;
</script>

<div class="counter-group">
  <!-- Leaf Icon -->
  <img src="/images/leaf-icon.svg" alt="leaf icon" class="icon" />

  <!-- Dynamic title "X Free" -->
  <h3 class="counter-title">{counter.name} Free</h3>

  <!-- Counter Button -->
  <button class="counter-button" on:click={() => incrementCounter(index)}>
    {counter.count} days
  </button>

  <!-- Counter name input and controls -->
  <div class="controls">
    <input
      type="text"
      placeholder="Enter name"
      bind:value={counter.name}
      class="input"
    />
    <button class="button save" on:click={() => saveCounterName(index, counter.name)}>Save</button>
    <button class="button reset" on:click={() => resetCounter(index)}>Reset</button>
    {#if index > 0}
      <button class="button delete" on:click={() => deleteCounter(index)}>Delete</button>
    {/if}
  </div>

  <!-- Last Incremented Text -->
  <p class="small-text">Last Incremented: {counter.lastIncrement || "Never"}</p>
</div>

<style>
  .counter-group {
    text-align: center;
    margin: 20px 0;
    background: rgba(255, 255, 255, 0.85);
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    border: 2px solid #3182ce;
  }

  .icon {
    width: 30px;  /* Adjust size as needed */
    margin-bottom: 10px;
    opacity: 0.7;
  }

  .counter-title {
    font-size: 1.2em;
    color: #4a5568;
    margin-bottom: 10px;
  }

  .counter-button {
    font-size: 2em;
    padding: 10px 20px;
    margin: 10px 0;
    background-color: #3182ce;
    color: white;
    border: none;
    border-radius: 10px;
    cursor: pointer;
  }

  .controls {
    display: flex;
    justify-content: center;
    gap: 5px;
  }

  .input {
    width: 80px;
    padding: 5px;
    text-align: center;
  }

  .button {
    padding: 5px 10px;
    font-size: 0.9em;
    border-radius: 8px;
    cursor: pointer;
  }

  .save { background-color: #edf2f7; }
  .reset { background-color: #e2e8f0; }
  .delete { background-color: #fccaca; }

  .small-text {
    font-size: 0.9em;
    color: #718096;
  }
</style>