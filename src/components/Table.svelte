<script lang="ts">
  import { brl } from '$lib';
  import { getContext } from 'svelte';
  import { Pagination, type ToastContext } from '@skeletonlabs/skeleton-svelte';

  import Icon from "@iconify/svelte";
  import Modal from './Modal.svelte';
  import Input from './ui/Input.svelte';

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
        price: number;
    }

    // Mocked data
    const originalSourceData: SourceData[] = [
        { code: 1, description: 'Hydrogen', unit: "UN", price: 10.0 },
        { code: 2, description: 'Helium', unit: "UN", price: 10.0 },
        { code: 3, description: 'Lithium', unit: "UN", price: 10.0 },
        { code: 4, description: 'Beryllium', unit: "UN", price: 10.0 },
        { code: 5, description: 'Boron', unit: "UN", price: 10.0 },
        { code: 6, description: 'Carbon', unit: "UN", price: 10.0 },
        { code: 7, description: 'Nitrogen', unit: "UN", price: 10.0 },
        { code: 8, description: 'Oxygen', unit: "UN", price: 10.0 },
        { code: 9, description: 'Fluorine', unit: "UN", price: 10.0 },
        { code: 10, description: 'Neon', unit: "UN", price: 10.0 }
    ];

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
    const openProductModal = (row: SourceData) => {
        productModalData = row;
        isOpen = true;
    };

    let quantity = $state(1);
    let profitMargin = $state(0);
</script>

<!-- Table -->
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
            <td class="text-right">{brl(row.price)}</td>
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

<!-- Products Modal -->
<Modal {isOpen}>
    <div class="relative flex flex-col gap-6 w-2/3 min-h-80 rounded-md p-6 bg-surface-900">
        <!-- Título -->
        <h2 class="text-xl font-semibold text-white">Adicionar Produto ao Carrinho</h2>

        <!-- Main info -->
        <div class="flex gap-6">
            <div class="flex-1 flex flex-col gap-4">
                <div>
                    <span class="text-sm text-zinc-300">Descrição</span>
                    <span class="text-xl font-semibold">{productModalData.description}</span>
                </div>
                <div>
                    <span class="text-sm text-zinc-300">Preço</span>
                    <span class="text-xl font-semibold text-green-400">{brl(productModalData.price)}</span>
                </div>
                <div class="flex gap-4">
                    <Input label="Quantidade" bind:value={quantity}/>
                    <Input label="Margem de lucro %" bind:value={profitMargin}/>
                </div>
            </div>
            <div class="flex-1 flex flex-col justify-end gap-4">
                <div class="bg-zinc-800 p-3 rounded-md">
                    <span class="text-sm text-zinc-300">Você compra por</span>
                    <span class="text-xl font-semibold text-yellow-400">{brl(productModalData.price * quantity)}</span>
                </div>
                <div class="bg-zinc-800 p-3 rounded-md">
                    <span class="text-sm text-zinc-300">Seu cliente paga</span>
                    <span class="text-xl font-semibold text-green-400">{brl(productModalData.price * quantity * (1 + profitMargin/100))}</span>
                </div>
            </div>
        </div>

        <!-- Action button -->
        <button class="mt-auto bg-tertiary-600 hover:bg-tertiary-500 text-white font-semibold py-2 px-4 rounded-md">
            Adicionar ao Orçamento
        </button>

        <!-- Close button -->
        <button onclick={() => isOpen = false} class="absolute top-4 right-4 w-10 h-10 flex items-center justify-center rounded-full hover:bg-zinc-700 transition">
            <Icon icon="lucide:octagon-x" font-size="24" class="text-zinc-400 hover:text-zinc-300"/>
        </button>
    </div>
</Modal>
