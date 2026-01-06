<script lang="ts">
let val = 0;
const incHandler = () => { val++; }
</script>

<h1>This is project <span class="mono">inc-button</span></h1>
<p>Enter a value or start clicking on the button</p>

<button on:click={incHandler}>Increment</button>
<!-- You can also inline the function: -->
<!-- <button on:click={() => { val++; }}>Increment</button> -->

<!-- with this construction, the user can not enter a new value -->
<!-- <input type="number" value={val} /> -->
<!-- with this construction, the user can enter a new value -->
<input type="number" bind:value={val} />

<style>
.mono { font-family: Courier; }
</style>
