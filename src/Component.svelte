<script>
  import { getContext } from "svelte"

  export let heading
  export let headingSize
  export let headingWeight
  export let headingColor
  export let headingVariant
  export let subHeading
  export let hideHeading
  export let dataProvider
  export let fieldList
  export let direction
  export let columns
  export let rowGap
  export let columnGap
  export let collapsible
  export let collapsed
  export let labelsAbove
  export let labelWidth
  export let labelSize
  export let labelWeight
  export let labelColor
  export let labelVariant
  export let valueSize
  export let valueColor
  export let valueWeight
  export let valueVariant

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  $: row = dataProvider?.rows.length > 0 ? dataProvider.rows[0] : {}
</script>

<div use:styleable={$component.styles}>
  {#if !hideHeading}
    <div 
      class="super-field-view-heading spectrum-Heading spectrum-Heading--size{headingSize}"  
      style:--super-field-view-heading-color={headingColor} > {heading} </div>
  {/if}
  <div class="superFieldView" 
    style:--super-field-view-label-color={labelColor}

    style:--super-field-view-row-gap={rowGap}
    style:--super-field-view-column-gap={columnGap}
    style:--super-field-view-columns={columns}>

    {#if !fieldList }
      Please select some fields
    {:else if !row}
      No Data Found
    {:else}
      {#each fieldList as field, idx}
      <div 
        style:--super-field-view-item-direction={labelsAbove ? "column" : "row"}
        class="superFieldViewItem">

          <div 
            style:--super-field-view-label-width={labelWidth}
            class:spectrum-Heading--light={labelWeight=="light"}
            class="superFieldViewItemLabel spectrum-Heading spectrum-Heading--size{labelSize}"> 
            {field.displayName.toUpperCase()} 
          </div>

          <div class="superFieldViewItemValue spectrum-Body spectrum-Body--size{valueSize}">
            { row[field.name] || "-" } 
          </div>

      </div>
      {/each}
      <slot />
    {/if}
  </div>
</div>

<style>

  .super-field-view-heading {
    color: var(--super-field-view-heading-color);
    margin-bottom: 1.25rem;
  }

  .superFieldView {
    display: grid;
    row-gap: var(--super-field-view-row-gap);
    column-gap: var(--super-field-view-column-gap);
    grid-template-columns: repeat(var(--super-field-view-columns), 1fr);
    width: 100%;
  }

  .superFieldViewItem {
    display: flex;
    flex-direction: var(--super-field-view-item-direction);
    justify-items: center;
    row-gap: 0.25rem;
    column-gap: 0.85rem;
  }

  .superFieldViewItemLabel {
    min-width: var(--super-field-view-label-width);
    color: var(--super-field-view-label-color);
    display: flex;
    flex-direction: column;
    justify-content: center;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis !important;
  }
  .superFieldViewItemValue {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis !important;
  }
</style>
