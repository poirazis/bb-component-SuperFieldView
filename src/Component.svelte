<script>
  import { getContext } from "svelte"

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  export let heading
  export let headingSize
  export let headingWeight
  export let headingColor
  export let hideHeading
  export let dataProvider
  export let fieldList
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
  export let valueSize
  export let valueColor

  $: row = dataProvider?.rows?.length > 0 ? dataProvider.rows[0] : {}

  // Add CSS variables to outer wrapper
  $: $component.styles.normal = { 
    ...$component.styles.normal, 
    "--super-field-view-row-gap" : rowGap,
    "--super-field-view-column-gap" : columnGap,
    "--super-field-view-columns" : columns,
    "--super-field-view-label-color" : labelColor,
    "--super-field-view-value-color" : valueColor,
    "--super-field-view-heading-color" : headingColor,
    "--super-field-view-item-direction" : labelsAbove ? "column" : "row",
    "--super-field-view-label-width" : labelWidth
  } 

</script>

<div use:styleable={$component.styles}>

  {#if collapsible}

    <div class="spectrum-Accordion">
      <div class:is-open={!collapsed} class="spectrum-Accordion-item">
        <h3 class="spectrum-Accordion-itemHeading">
          <button 
            on:click={() => collapsed = !collapsed } 
						class:spectrum-Heading--light={headingWeight == "light"}
            class="spectrum-Accordion-itemHeader spectrum-Heading spectrum-Heading--size{headingSize}" type="button">{heading.toUpperCase()}
          </button>
          <svg
          class="spectrum-Icon spectrum-UIIcon-ChevronRight100 spectrum-Accordion-itemIndicator" focusable="false" aria-hidden="true">
            <use xlink:href="#spectrum-css-icon-Chevron100" />
          </svg>
        </h3>
        <div class="spectrum-Accordion-itemContent">
          <div class="superFieldView">
            {#each fieldList as field, idx}
              <div class="superFieldViewItem">
        
                  <div 
                    class:spectrum-Heading--light={labelWeight=="light"}
                    class="superFieldViewItemLabel spectrum-Heading spectrum-Heading--size{labelSize}"> 
                    {field.displayName.toUpperCase()} 
                  </div>
        
                  <div class="superFieldViewItemValue spectrum-Body spectrum-Body--size{valueSize}">
                    { row[field.name] || "-" } 
                  </div>
        
              </div>
            {/each}
          </div>
        </div>
      </div>
    </div>

  {:else}
	{#if !hideHeading}
    <h3 class="spectrum-Heading--size{headingSize} super-field-view-heading" class:spectrum-Heading--light={headingWeight == "light"} >
        {heading.toUpperCase()}
    </h3>
		{/if}
		<div class="superFieldView">
			{#each fieldList as field, idx}
				<div class="superFieldViewItem">
	
						<div 
							class:spectrum-Heading--light={labelWeight=="light"}
							class="superFieldViewItemLabel spectrum-Heading spectrum-Heading--size{labelSize}"> 
							{field.displayName.toUpperCase()} 
						</div>
	
						<div class="superFieldViewItemValue spectrum-Body spectrum-Body--size{valueSize}">
							{ row[field.name] || "-" } 
						</div>
	
				</div>
			{/each}
		</div>

  {/if}
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
    display: flex;
    flex-direction: column;
    justify-content: center;
    white-space: nowrap;
    min-width: var(--super-field-view-label-width);
    color: var(--super-field-view-label-color);
  }

  .superFieldViewItemValue {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    white-space: nowrap;
    color: var(--super-field-view-value-color);
  }

	.spectrum-Accordion-itemContent {
		padding-top: 0.85rem;
	}

  .spectrum-Accordion-itemHeading .spectrum-Accordion-itemHeader {
    min-height: unset !important;
		color: var(--super-field-view-heading-color)
  }

  .spectrum-Accordion-itemHeading .spectrum-Accordion-itemHeader:hover {
		color: var(--spectrum-global-color-gray-900);
  }
</style>
