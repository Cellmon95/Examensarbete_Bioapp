<script lang="ts">
	import '../styles.css';
	import { invalidate } from '$app/navigation';
	import { onMount } from 'svelte';

	export let data;

	let { supabase, session } = data;
	$: ({ supabase, session } = data);

	onMount(() => {
		const {
			data: { subscription }
		} = supabase.auth.onAuthStateChange((event, _session) => {
			if (_session?.expires_at !== session?.expires_at) {
				invalidate('supabase:auth');
			}
		});
		console.log(session);
		return () => subscription.unsubscribe();
	});
</script>

<header>
	<div class="home-link">
		<a href="/">
			<span class="material-symbols-outlined"> home </span>
		</a>
	</div>
	<div class="auth-link">
		{#if session}
			<a href="/account">
				<span class="material-symbols-outlined"> account_circle </span>
			</a>
		{:else}
			<a href="/login">
				<span class="material-symbols-outlined"> login </span>
			</a>
		{/if}
	</div>
</header>

<main>
	<slot />
</main>

<footer>
	<!--TODO:: Add a footer-->
</footer>

<style>
	header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		height: 40px;
		background-color: red;
		padding: 20px;
	}

	.home-link span {
		font-size: 40px;
	}

	.auth-link span {
		font-size: 40px;
	}
</style>
