<script>
    export let isOpen = false;
</script>

<div class={`${isOpen ? "visible bg-black/50" : "hidden"} fixed inset-0 w-screen h-screen flex items-center justify-center z-50`}>
    <slot />
</div>
