<script context="module" lang="ts">
  import { cloudstate, useCloudState } from "freestyle-sh";

  @cloudstate
  export class CloudCounter {
    value = 0;
    increment() {
      return ++this.value;
    }
    getCount() {
      return this.value;
    }
  }
</script>

<script lang="ts">
  export let count = 0;

  const counter = useCloudState(CloudCounter);
</script>

<button
  on:click={() => {
    counter.increment().then((c) => {
      count = c;
    });
  }}
>
  this button has been clicked {count} times
</button>

<style>
  button {
    padding: 0.5rem 1rem;
    font-size: 1rem;
    background-color: #0070f3;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  button:hover {
    background-color: #0053b3;
  }
</style>
