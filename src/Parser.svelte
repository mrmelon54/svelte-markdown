<script>
  import { supressWarnings } from './supress-warnings'

  export let type = undefined
  export let tokens = undefined
  export let header = undefined
  export let rows = undefined
  export let ordered = false
  export let renderers
  export let options

  supressWarnings();
</script>

{#if !type}
  {#each tokens as token}
    <svelte:self {...token} {renderers} {options} />
  {/each}
{:else}
  {#if renderers[type]}
    {#if type === 'table'}
      <svelte:component this={renderers.table} {options}>
        <svelte:component this={renderers.tablehead} {options}>
          <svelte:component this={renderers.tablerow} {options}>
            {#each header as headerItem, i}
              <svelte:component
                this={renderers.tablecell}
                header={true}
                align={$$restProps.align[i] || 'center'}
                {options}
                >
                <svelte:self tokens={headerItem.tokens} {renderers} {options} />
              </svelte:component>
            {/each}
          </svelte:component>
        </svelte:component>
        <svelte:component this={renderers.tablebody} {options}>
          {#each rows as row}
            <svelte:component this={renderers.tablerow} {options}>
              {#each row as cells, i}
                <svelte:component
                  this={renderers.tablecell}
                  header={false}
                  align={$$restProps.align[i] || 'center'}
                  {options}
                  >
                  <svelte:self tokens={cells.tokens} {renderers} {options} />
                </svelte:component>
              {/each}
            </svelte:component>
          {/each}
        </svelte:component>
      </svelte:component>
    {:else if type === 'list'}
      {#if ordered}
        <svelte:component this={renderers.list} {ordered} {options} {...$$restProps}>
          {#each $$restProps.items as item}
            <svelte:component this={renderers.orderedlistitem || renderers.listitem} {options} {...item}>
              <svelte:self tokens={item.tokens} {renderers} {options} />
            </svelte:component>
          {/each}
        </svelte:component>
      {:else}
        <svelte:component this={renderers.list} {ordered} {options} {...$$restProps}>
          {#each $$restProps.items as item}
            <svelte:component this={renderers.unorderedlistitem || renderers.listitem} {options} {...item}>
              <svelte:self tokens={item.tokens} {renderers} {options} />
            </svelte:component>
          {/each}
        </svelte:component>
      {/if}
    {:else}
      <svelte:component this={renderers[type]} {options} {...$$restProps}>
        {#if tokens}
          <svelte:self {tokens} {renderers} {options} />
        {:else}
          {$$restProps.raw}
        {/if}
      </svelte:component>
    {/if}
  {/if}
{/if}
