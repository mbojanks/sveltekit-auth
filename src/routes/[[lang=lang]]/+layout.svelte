<script lang="ts">
	import '../../app.pcss';
	import { page } from '$app/stores';
	import { ModeWatcher } from 'mode-watcher';
	import { getFlash } from 'sveltekit-flash-message';
	import { Toaster } from '$lib/components/ui/sonner';
	import { toast } from 'svelte-sonner';
	import Navigation from '$lib/components/navigation/navigation.svelte';
	import { type AvailableLanguageTag } from '$paraglide/runtime';
	import { defaultLanguage } from '$lib/i18n-routing';

	export let data: any;
	let user: Lucia.UserAttributes;
	$: user = data.user;
	let currLang: AvailableLanguageTag;
	$: currLang = $page.params.lang as AvailableLanguageTag || defaultLanguage /* data.lang not updated ? */;
	const flash = getFlash(page);
	//$: console.log('+layout.svelte root flash: ' + JSON.stringify($flash));
	$: if ($flash) {
		switch ($flash.type) {
			case 'success':
				//console.log('flash.message.success: ' + $flash.message);
				toast.success($flash.message);
				break;
			case 'error':
				//console.log('flash.message.error: ' + $flash.message);
				toast.error($flash.message);
				break;
		}
	}
</script>

<ModeWatcher />
<Toaster richColors />
<div class="relative flex min-h-screen flex-col">
	<Navigation {user} lang={currLang} />
	<div class="mt-8 md:mt-12">
		{#key $page.data.user}
			<slot />
		{/key}
	</div>
</div>
