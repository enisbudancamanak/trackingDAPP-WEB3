<script lang="ts">
	import { onMount } from 'svelte';
	import { web3, defaultEvmStores, connected, selectedAccount } from 'svelte-web3';

	onMount(async () => {});

	const enableBrowser = () => {
		defaultEvmStores.setBrowserProvider();

		// var body = document.querySelector('body');
		// for (var i = 0; i < 60; i++) {
		// 	var el = jazzicon(100, Math.round(Math.random() * 10000000));
		// 	body.appendChild(el);
		// }
	};

	const logout = () => {
		defaultEvmStores.disconnect();
	};
</script>

<!-- <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
	<div class="container-fluid">
		<a class="navbar-brand" href="#">Navbar</a>
		<button
			class="navbar-toggler"
			type="button"
			data-bs-toggle="collapse"
			data-bs-target="#navbarSupportedContent"
			aria-controls="navbarSupportedContent"
			aria-expanded="false"
			aria-label="Toggle navigation"
		>
			<span class="navbar-toggler-icon" />
		</button>
		<div class="collapse navbar-collapse" id="navbarSupportedContent">
			<ul class="navbar-nav me-auto mb-2 mb-lg-0">
				<li class="nav-item">
					<a class="nav-link active" aria-current="page" href="#">Home</a>
				</li>
				<li class="nav-item">
					<a class="nav-link" href="#">Link</a>
				</li>
				<li class="nav-item dropdown">
					<a
						class="nav-link dropdown-toggle"
						href="#"
						id="navbarDropdown"
						role="button"
						data-bs-toggle="dropdown"
						aria-expanded="false"
					>
						Dropdown
					</a>
					<ul class="dropdown-menu" aria-labelledby="navbarDropdown">
						<li><a class="dropdown-item" href="#">Action</a></li>
						<li><a class="dropdown-item" href="#">Another action</a></li>
						<li><hr class="dropdown-divider" /></li>
						<li><a class="dropdown-item" href="#">Something else here</a></li>
					</ul>
				</li>
				<li class="nav-item">
					<a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
				</li>
			</ul>
			<form class="d-flex">
				<button class="btn btn-outline-info" type="submit"
					>{#if $web3.version}
						{#if $connected}
							<button id="connectButton" type="button" class="btn btn-info"
								>connected
							</button>{:else}
							<button id="connectButton" type="button" class="btn btn-info" on:click={enableBrowser}
								>connect to the browser window provider (eg Metamask)
							</button>
						{/if}
					{/if}</button
				>
			</form>
		</div>
	</div>
</nav> -->

<nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top">
	<div class="container-xl">
		<a class="navbar-brand link" href="/searchEntries">All Entries</a>

		<div class="navbar-collapse collapse w-100 order-1 order-md-0 dual-collapse2">--></div>
		<div class="mx-auto order-0">
			<a class="navbar-brand mx-auto" href="/">Fish Entry System using Blockchain</a>
			<button
				class="navbar-toggler"
				type="button"
				data-toggle="collapse"
				data-target=".dual-collapse2"
			>
				<span class="navbar-toggler-icon" />
			</button>
		</div>
		<div class="navbar-collapse collapse w-100 order-3 dual-collapse2">
			<ul class="navbar-nav ms-auto">
				<li class="nav-item" />
			</ul>
		</div>
	</div>
	<div style="position: absolute; z-index: 99; right: 0; padding-right: 20px !important;">
		{#if $web3.version}
			{#if $connected}
				<button id="connectButton" type="button" class="btn btn-info" on:click={logout}
					>logout
				</button>

				<span class="text-light">
					connected as <span class="border border-light rounded"
						><img
							height="40"
							src="https://cohire-image-bucket-2.s3.amazonaws.com/company/6216/-1609466507-identicon.png"
							alt=""
						/>
						{$selectedAccount}
					</span></span
				>
			{:else}
				<button id="connectButton" type="button" class="btn btn-info" on:click={enableBrowser}
					>connect to the browser window provider (eg Metamask)
				</button>
			{/if}
		{/if}
	</div>
</nav>

<style>
	a:hover {
		color: rgb(224, 224, 224) !important;
		text-decoration: underline;
	}
</style>
