<script lang="ts">
  import { getOverlayDocumentType } from './utils';
  import { DocumentType } from '../../contexts/app-state';
  import { configuration as globalConfiguration } from '../../contexts/configuration';
  import { makeStylesFromConfiguration } from '../../utils/css-utils';
  import merge from 'lodash.merge';
  import { overlay } from '../../default-configuration/theme';

  export let type: DocumentType;

  const overlayType = getOverlayDocumentType(type);

  const style = makeStylesFromConfiguration(merge(overlay, $globalConfiguration.overlay));
</script>

<div class="overlay {overlayType}" />

<style>
  .overlay {
    z-index: 1;
    position: relative;
    margin: 10px 0px 0px 0px;
    max-width: 80%;
    max-height: 100%;
    outline: 1200px solid #000000d6;
    border: 3px solid #ffffff4d;
    width: 100%;
  }
  .overlay.card {
    border-radius: 12px;
    aspect-ratio: 4/3;
  }
  .overlay.passport {
    border-radius: 12px;
    aspect-ratio: 3/4;
  }
  .overlay.a4 {
    border-radius: 12px;
    aspect-ratio: 3/4;
  }
  .overlay.selfie {
    border-radius: 100%;
    aspect-ratio: 3/4;
  }
</style>
