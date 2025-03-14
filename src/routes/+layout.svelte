<script>
	import '../app.css';
	let { children } = $props();
	import { ToastProvider } from '@skeletonlabs/skeleton-svelte';
</script>

<ToastProvider>
	{@render children()}
</ToastProvider>
