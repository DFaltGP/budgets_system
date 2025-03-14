<script lang="ts">
  import { getContext } from 'svelte';
  import { Pagination, type ToastContext } from '@skeletonlabs/skeleton-svelte';

  import Icon from "@iconify/svelte";
  import Modal from './Modal.svelte';

  export const toast: ToastContext = getContext('toast');

  function triggerSuccess() {
    toast.create({ 
        type: "success", 
        title: "Importação concluída",
        description: 'Produtos importados com sucesso'
    });
  }

  function triggerError() {
    toast.create({
        type: "error",
        title: "Importação falhou",
        description: "Ocorreu um erro ao importar produtos"
    })
  }

    interface SourceData {
        code: number;
        description: string;
        unit: string;
        price: string;
    }

    // Mocked data
    let originalSourceData: SourceData[] = $state([
        { code: 1, description: 'Hydrogen', unit: "UN", price: 'R$ 10,00' },
        { code: 2, description: 'Helium', unit: "UN", price: 'R$ 10,00' },
        { code: 3, description: 'Lithium', unit: "UN", price: 'R$ 10,00' },
        { code: 4, description: 'Beryllium', unit: "UN", price: 'R$ 10,00' },
        { code: 5, description: 'Boron', unit: "UN", price: 'R$ 10,00' },
        { code: 6, description: 'Carbon', unit: "UN", price: 'R$ 10,00' },
        { code: 7, description: 'Nitrogen', unit: "UN", price: 'R$ 10,00' },
        { code: 8, description: 'Oxygen', unit: "UN", price: 'R$ 10,00' },
        { code: 9, description: 'Fluorine', unit: "UN", price: 'R$ 10,00' },
        { code: 10, description: 'Neon', unit: "UN", price: 'R$ 10,00' }
    ]);

    let sourceData = $state([...originalSourceData]);

    // State
    let page = $state(1);
    let size = $state(5);
    const slicedSource = $derived((s: SourceData[]) => s.slice((page - 1) * size, page * size));

    const filterProducts = (e) => {
        const searchTerm = e.target.value.toLowerCase();
        sourceData = originalSourceData.filter((item) =>item.description.toLowerCase().includes(searchTerm));
    };

    const importData = () => {
        try {
            triggerSuccess();
        } catch (_error) {
            triggerError();
        }
    };

    let isOpen = $state(false);
    let productModalData: SourceData = $state({} as SourceData);
    const openProductModal = (row) => {
        productModalData = row;
        isOpen = true;
    };
</script>
  
<section class="space-y-4 rounded-lg p-12 border border-zinc-700 shadow-md">
<!-- Tools Bar -->
<div class="w-full flex items-center justify-between gap-4">
    <input 
        type="text" 
        placeholder="Filtrar produtos" 
        oninput={(e) => filterProducts(e)}
        class="p-2 w-96 border border-zinc-700 outline-none focus:ring-1 focus:ring-white rounded-md"
    >
    <h1 class="flex flex-grow text-4xl font-light ml-32">Tabela de produtos</h1>
    <button aria-label="Importar produtos" onclick={importData} class="flex items-center justify-center p-3 border-2 border-zinc-700 hover:bg-zinc-800 transition-all  rounded-md">
        <Icon icon="lucide:import" />
    </button>
</div>
<!-- Table -->
<div class="table-wrap">
    <table class="table table-fixed caption-bottom">
    <thead>
        <tr>
        <th>Código</th>
        <th>Descrição</th>
        <th>Unidade</th>
        <th class="!text-right">Preço</th>
        </tr>
    </thead>
    <tbody class="[&>tr]:hover:preset-tonal-primary">
        {#each slicedSource(sourceData) as row}
        <tr onclick={() => openProductModal(row)}>
            <td>{row.code}</td>
            <td>{row.description}</td>
            <td>{row.unit}</td>
            <td class="text-right">{row.price}</td>
        </tr>
        {/each}
    </tbody>
    </table>
</div>
<!-- Footer -->
<footer class="flex items-end justify-between">
    <select name="size" id="size" class="select max-w-[150px] h-10" value={size} onchange={(e) => (size = Number(e.currentTarget.value))}>
    {#each [1, 2, 5] as v}
        <option value={v}>{v} Itens</option>
    {/each}
    <option value={sourceData.length}>Todos</option>
    </select>
    <!-- Pagination -->
    <Pagination
    data={sourceData}
    {page}
    onPageChange={(e) => (page = e.page)}
    pageSize={size}
    onPageSizeChange={(e) => (size = e.pageSize)}
    siblingCount={4}
    >
    {#snippet labelEllipsis()}<Icon icon="lucide:ellipsis" />{/snippet}
    {#snippet labelNext()}<Icon icon="lucide:arrow-big-right" />{/snippet}
    {#snippet labelPrevious()}<Icon icon="lucide:arrow-big-left" class="size-4" />{/snippet}
    {#snippet labelFirst()}<Icon icon="lucide:chevron-first"  />{/snippet}
    {#snippet labelLast()}<Icon icon="lucide:chevron-last" />{/snippet}
    </Pagination>
</footer>
</section>

<Modal isOpen={isOpen}>
    <div class="relative flex flex-col gap-4 w-1/2 h-96 rounded-md p-4 bg-zinc-900">
    <span>Este é o modal de produtos</span>
        <button class="absolute top-4 right-4" onclick={() => isOpen = false}>
            <Icon icon="lucide:octagon-x" font-size="24" class="hover:text-zinc-300 transition-colors"/>
        </button>
    </div>
</Modal>