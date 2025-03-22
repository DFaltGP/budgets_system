<script>
	import '../app.css';
	let { children } = $props();
	
	import { ToastProvider } from '@skeletonlabs/skeleton-svelte';
  	import AppRail from '../components/AppRail.svelte';
</script>

<ToastProvider>
	<AppRail>
		{@render children()}
	</AppRail>
</ToastProvider>
