<script lang="ts">
  import { SvelteToast } from '@zerodevx/svelte-toast';
  import { initConnectionCheck } from './lib/utils/check-connection';
  import { configuration } from './lib/contexts/configuration';
  import { currentLanguage, Languages } from './lib/contexts/translation';
  import { subscribe } from './lib/utils/event-service';
  import Routes from './Routes.svelte';
  import { t } from './lib/contexts/translation/hooks';
  import { setFlowName } from './lib/contexts/flows';

  subscribe();
  initConnectionCheck(t);

  const urlParams = new URLSearchParams(window.location.search);

  // const flowName = urlParams.get("b_fid");
  export let flowName;
  setFlowName($configuration.flows, flowName);

  // Omri - query params task:
  $currentLanguage =
    ($configuration.endUserInfo.language as Languages) || $configuration.defaultLanguage;

  const style = `
    --font-family: ${$configuration.general.fonts.name};
    --primary-color: ${$configuration.general.colors.primary};
  `;
</script>

<svelte:head>
  {#if $configuration.general.fonts.link}
    <link href={$configuration.general.fonts.link} rel="stylesheet" />
  {/if}
</svelte:head>

<main {style}>
  <!-- <button on:click={handle}>Change</button> -->
  <Routes />
  <SvelteToast
    options={{
      theme: {
        '--toastBackground': $configuration.general.colors.primary,
        '--toastBarBackground': $configuration.general.colors.primary,
      },
    }}
  />
</main>

<style>
  main {
    max-width: 500px;
    width: 100%;
    margin: 0 auto;
    height: 100%;
  }

  :global(html, body, #app) {
    height: 100%;
  }

  :global(html, body, p, div, input) {
    box-sizing: border-box;
  }

  :global(body) {
    margin: 0px;
  }

  :global(p, div, body, button) {
    font-family: var(--font-family);
  }
</style>
