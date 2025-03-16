<script>
  let { label } = $props();
  let inputFocus = $state(false);
</script>

<div class="relative">
  <span class={`${inputFocus ? "-top-3 text-zinc-100 text-sm" : "top-2 text-zinc-500 text-base"} absolute left-3 transition-all bg-surface-900 px-1`}>{label}</span>
  <input
    type="text" 
    class={`${inputFocus ? "ring-primary-950" : "ring-surface-900"} w-full border border-zinc-500 p-2 rounded-md outline-none ring-2`}
    onfocus={() => inputFocus = true}
    onblur={(e) => inputFocus = e.target.value != ""}
    >
  </div>