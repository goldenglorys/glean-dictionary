<script>
  import { isUndefined } from "lodash";
  import HelpHoverable from "./HelpHoverable.svelte";

  export let appName = "";
  export let schema = [];
  export let item = {};

  function format(ref, formatter) {
    return formatter ? formatter(ref, appName) : ref;
  }
</script>

<style>
  @import "../main.scss";
  @include metadata-table;
</style>

<table>
  <col />
  <col />
  {#each schema as schemaEntry}
    {#if !isUndefined(item[schemaEntry.id])}
      <tr>
        <td>
          {schemaEntry.title}
          {#if schemaEntry.helpText}
            <HelpHoverable
              content={schemaEntry.helpText}
              link={schemaEntry.helpLink} />
          {/if}
        </td>
        <td>
          {#if schemaEntry.type === 'link'}
            <a
              href={format(item[schemaEntry.id], schemaEntry.linkFormatter)}>{format(item[schemaEntry.id], schemaEntry.valueFormatter)}</a>
          {:else if schemaEntry.type === 'links'}
            {#each item[schemaEntry.id] as ref}
              <div>
                <a
                  href={format(ref, schemaEntry.linkFormatter)}>{format(ref, schemaEntry.valueFormatter)}</a>
              </div>
            {/each}
          {:else}{format(item[schemaEntry.id], schemaEntry.valueFormatter)}{/if}
        </td>
      </tr>
    {/if}
  {/each}
  <slot />
</table>
