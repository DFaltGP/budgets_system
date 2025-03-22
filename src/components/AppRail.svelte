<script lang="ts">
    import { Navigation } from '@skeletonlabs/skeleton-svelte';
    import Icon from '@iconify/svelte';

    let value = $state("/");
  </script>
  
  <div class="card border border-surface-100-900 grid h-screen w-full grid-cols-[auto_1fr]">
    <!-- Component -->
    <Navigation.Rail {value} onValueChange={(newValue) => (value = newValue)} classes="border-r border-surface-700 shadow-xs shadow-surface-700">
      {#snippet header()}
        <Navigation.Tile href="/" title="Home"><Icon icon="lucide:home" /></Navigation.Tile>
      {/snippet}
      {#snippet tiles()}
        <Navigation.Tile label="Produtos" href="/products"><Icon icon="lucide:boxes"/></Navigation.Tile>
        <Navigation.Tile label="Orçamento" href="/budgets"><Icon icon="lucide:pen-box"/></Navigation.Tile>
        <Navigation.Tile label="Impressão" href="/prints"><Icon icon="lucide:printer"/></Navigation.Tile>
      {/snippet}
      {#snippet footer()}
        <Navigation.Tile labelExpanded="Settings" href="/settings" title="settings"><Icon icon="lucide:settings" /></Navigation.Tile>
      {/snippet}
    </Navigation.Rail>
    <!-- Content -->
    <div class="flex items-center justify-center overflow-y-auto">
        <slot />
    </div>
  </div>