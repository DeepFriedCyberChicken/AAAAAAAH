<script lang="ts">
  import AboutModal from "../lib/components/AboutModal.svelte";
  import SettingsModal from "../lib/components/SettingsModal.svelte";
  import { inject } from "@vercel/analytics";
  import InfiniteMode from "../lib/components/InfiniteMode.svelte";
  import { gameMode, isDaily } from "../lib/util/stores";
  import { browser } from "$app/environment";

  if (browser && sessionStorage.getItem("analytics") == null) {
    sessionStorage.setItem("analytics", "");
    try {
      inject();
    } catch {
      // ignored
    }
  }

  gameMode.subscribe((gm) => {
    if (gm == null) return;
    localStorage.setItem("gamemode", gm);
  });

  let aboutModal = false;
  let settingsModal = false;
</script>

{#if aboutModal}
  <AboutModal on:close={() => (aboutModal = false)} />
{/if}

{#if settingsModal}
  <SettingsModal on:close={() => (settingsModal = false)} />
{/if}

<header class="header">
  <h1><a data-sveltekit-reload href="/daily" class="header-emoji">♾️<div class="hover-bubble">Click to switch to daily mode</div></a> AniGuess</h1>
  <div class="meta-buttons">
    <button class="button-about" on:click={() => (aboutModal = true)}>
      <img src="info.svg" width="35" height="35" alt="" />
    </button>
    <button class="button-settings" on:click={() => (settingsModal = true)}>
      <img src="settings.svg" width="35" height="35" alt="" />
    </button>
  </div>
</header>
<InfiniteMode />

<style>
  .header-emoji {
    cursor: pointer;
  }

  .header-emoji:hover .hover-bubble {
    opacity: 1;
  }

  .hover-bubble {
    position: fixed;
    opacity: 0;
    background-color: #1a1a1a;
    color: white;
    transition: opacity 0.5s;
    font-size: 1.25rem;
    border-radius: 5px;
    padding: 3px;
  }


  .header {
    width: 450px;
    display: flex;
    justify-content: space-between;
    flex-direction: row;
    margin: 0 0;
    padding: 0 0;
  }

  h1 {
    align-self: center;
    color: white;
  }

  .meta-buttons {
    width: 110px;
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    align-items: center;
  }

  .meta-buttons button {
    align-self: center;
    justify-content: center;
    display: flex;
    width: 50px;
    height: 50px;
    border-radius: 15px;
    background-color: #1a1a1a;
    color: white;
  }

  .button-about img {
    align-self: center;
    filter: invert() opacity(0.75);
  }

  .button-settings img {
    align-self: center;
    filter: invert() opacity(0.75);
  }
</style>
