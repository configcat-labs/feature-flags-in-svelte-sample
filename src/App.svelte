<script>
  import * as configcat from "configcat-js";
  import { onMount } from "svelte";
  // Fact object
  export let data;
  export let featureFlag;

  // Fetch data once the component mounts
  onMount(async () => {
    const res = await fetch(`https://catfact.ninja/fact`);
    data = await res.json();

    // Connect to ConfigCat client
    const configCatClient = configcat.createClient(
      "fK7ZCApWbkaDu14njPKZQw/s9XWupU5K0KRp_9PvkU02g"
    );

    // Get feature flags value
    featureFlag = await configCatClient.getValueAsync("randomfactflag", false);
  });
</script>

<main>
  <h1>Random Fact About Cats</h1>

  <!-- Show fact only if flag is toggled on -->
  {#if featureFlag}
    <p>{data?.fact}</p>
  {/if}

  <!-- Show message if flag is toggled off -->
  {#if !featureFlag}
    <p>Ops! This feature is currently unavailable</p>
  {/if}
</main>

<style>
  h1 {
    font-size: 1.5rem;
  }
</style>
