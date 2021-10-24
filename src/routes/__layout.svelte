<script>
	import "../app.css";
	import Header from '$lib/header.svelte'
	import Footer from '$lib/footer.svelte'

	import { onMount } from 'svelte'
	import { themeChange } from 'theme-change'
	import { SvelteToast } from '@zerodevx/svelte-toast'

	onMount(async () => {
		themeChange(false)
	})
	
	</script>
	<Header />
	
	<slot></slot>
	<SvelteToast/>


	<Footer />