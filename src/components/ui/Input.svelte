<script>
  let { label, value = $bindable() } = $props();
  let inputFocus = $state(false);
</script>

<label class="relative">
  <span class={`${inputFocus ? "-top-3 text-zinc-100" : "top-2 text-zinc-500"} absolute left-2 transition-all bg-surface-900 px-1`}>{label}</span>
  <input
    bind:value
    type="number" 
    class={`${inputFocus ? "ring-primary-950" : "ring-surface-900"} w-full border border-zinc-500 p-2 rounded-md outline-none ring-2`}
    onfocus={() => inputFocus = true}
    onblur={(e) => inputFocus = e.target.value != ""}
    >
</label>