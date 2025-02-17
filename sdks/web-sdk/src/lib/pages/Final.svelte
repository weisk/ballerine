<script lang="ts">
  import { onDestroy } from 'svelte';
  import { T } from '../contexts/translation';
  import { Image, Button, Title, Paragraph, IconButton } from '../atoms';
  import { configuration, Steps } from '../contexts/configuration';
  import { currentParams } from '../contexts/app-state';
  import { Elements } from '../contexts/configuration/types';
  import { makeStylesFromConfiguration } from '../utils/css-utils';
  import { flowApproved } from '../services/analytics';
  import { DecisionStatus, sendFlowCompleteEvent } from '../utils/event-service';
  import { addCloseToURLParams } from '../contexts/navigation/hooks';
  import merge from 'lodash.merge';
  import { finalStep, layout } from '../default-configuration/theme';

  console.log("Final")

  const step = merge(finalStep, $configuration.steps[Steps.Final]);

  const style = makeStylesFromConfiguration(merge(layout, $configuration.layout), step.style);

  flowApproved();

  const handleClose = () => {
    sendFlowCompleteEvent({ status: 'completed', idvResult: DecisionStatus.APPROVED });
    addCloseToURLParams();
  };

  onDestroy(() => {
    $currentParams = null;
  });
</script>

<div class="container" {style}>
  {#each step.elements as element}
    {#if element.type === Elements.IconButton}
      <IconButton configuration={element.props} />
    {/if}
    {#if element.type === Elements.Image}
      <Image configuration={element.props} />
    {/if}
    {#if element.type === Elements.Title}
      <Title configuration={element.props}>
        <T key="title" module="final" />
      </Title>
    {/if}
    {#if element.type === Elements.Paragraph}
      <Paragraph configuration={element.props}>
        <T key={element.props.context || 'description'} module="final" />
      </Paragraph>
    {/if}
    {#if element.type === Elements.Button}
      <Button on:click={handleClose} configuration={element.props}>
        <T key="button" module="final" />
      </Button>
    {/if}
  {/each}
</div>

<style>
  .container {
    display: flex;
    flex-direction: column;
    padding: var(--padding);
    position: relative;
    background: var(--background);
    text-align: center;
    height: 100%;
  }
</style>
