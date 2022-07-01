<script>
	import Nav from '../components/Nav.svelte';
	import { onMount } from 'svelte';
	import Flatpickr from 'svelte-flatpickr';
	import 'flatpickr/dist/flatpickr.css';
	import Swal from 'sweetalert2';

	import {
		defaultEvmStores,
		web3,
		selectedAccount,
		connected,
		chainId,
		chainData
	} from 'svelte-web3';

	const options = {
		enableTime: false
	};

	let sendAdress = '0x7b0d0b4f72b1D8D928e0Db6c3E01744c0B773DA2';

	// $: checkAccount = $selectedAccount || '0x0000000000000000000000000000000000000000';
	// $: balance = $connected ? $web3.eth.getBalance(checkAccount) : '';

	onMount(() => {});

	const sendTip = async (e) => {
		const data = new FormData(document.getElementById('form'));
		const inputTextsInJson = Object.fromEntries(data.entries());

		if (inputTextsInJson.id.length == 0) {
			inputTextsInJson.id = Math.random().toString(36).slice(2, 36);
			// @ts-ignore
			document.querySelector('#existingIDInput').value = inputTextsInJson.id;
		}

		const tx = await $web3.eth.sendTransaction({
			// gasPrice: $web3.utils.toHex($web3.utils.toWei('30', 'gwei')),
			gasLimit: $web3.utils.toHex('40000'),
			from: $selectedAccount,
			to: sendAdress,
			value: $web3.utils.toHex($web3.utils.toWei('1', 'finney')),
			data: $web3.utils.toHex(inputTextsInJson)
		});

		Swal.fire('The Internet?', 'That thing is still around?', 'success');

		Swal.fire({
			imageUrl:
				'https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=' + inputTextsInJson.id,
			imageHeight: 150,
			imageAlt: 'A tall image',
			icon: 'success',
			text: 'Fish has been saved in the Blockchain'
		});
	};
</script>

<svelte:head>
	<title>Fish Entry System using Blockchain</title>
</svelte:head>

<Nav />

<main>
	<!-- Navigation -->

	<div style="display: flex; align-items: center; height: 100vh;">
		<div class="col md-6 no-gutters">
			<div class="right">
				<div class="container">
					<div class="row no-gutters">
						<div class="col-lg-12 text-center">
							<div class="card-body">
								<h4 class="card-title">Fish Blockchain Registry</h4>
								<p class="card-text">
									Fill out this form to make an entry for your fish to upload it to the blockchain
								</p>
							</div>
						</div>
					</div>
				</div>

				<br />

				<div class="container">
					<form id="form" class="was-validated">
						<div class="row no-gutters form-group">
							<label class="col-sm-2">Existing ID?</label>
							<div class="col-sm-10">
								{#if $connected}
									<input name="id" id="existingIDInput" rows="1" class="form-control" required />
								{:else}
									<input rows="1" class="form-control" readonly />
								{/if}
							</div>
						</div>

						<br />

						<div class="row no-gutters form-group">
							<label class="col-sm-2">Caught</label>
							<div class="col-sm-10">
								{#if $connected}
									<Flatpickr
										options={{ enableTime: false }}
										name="caught"
										class="form-control bg-white"
									/>
								{:else}
									<input rows="1" class="form-control" readonly />
								{/if}
							</div>
						</div>

						<br />

						<div class="row no-gutters form-group">
							<label class="col-sm-2">Location</label>
							<div class="col-sm-10">
								{#if $connected}
									<input name="location" rows="1" class="form-control is-invalid" required />
								{:else}
									<input rows="1" class="form-control" readonly />
								{/if}
							</div>
						</div>

						<br />

						<div class="row no-gutters form-group">
							<label class="col-sm-2">Species</label>
							<div class="col-sm-10">
								{#if $connected}
									<input name="species" rows="1" class="form-control" required />
								{:else}
									<input rows="1" class="form-control" readonly />
								{/if}
							</div>
						</div>

						<br />

						<div class="row no-gutters form-group">
							<label class="col-sm-2">Gender</label>
							<div class="col-sm-10">
								{#if $connected}
									<input name="gender" rows="1" class="form-control" required />
								{:else}
									<input rows="1" class="form-control" readonly />
								{/if}
							</div>
						</div>

						<br />

						<div class="row no-gutters form-group">
							<label class="col-sm-2">Health Issues of Note</label>
							<div class="col-sm-10">
								{#if $connected}
									<textarea name="healthNote" rows="1" class="form-control" required />
								{:else}
									<textarea rows="1" class="form-control" readonly />
								{/if}
							</div>
						</div>

						<br />
					</form>
				</div>

				<br />

				{#if $connected}
					<div class="row no-gutters">
						<div class="col-lg-12 text-center">
							<div class="container">
								<input
									type="button"
									class="btn btn-primary btn-lg"
									value="Create Entry"
									on:click={sendTip}
								/>
							</div>
						</div>
					</div>
				{:else}
					<div class="row no-gutters">
						<div class="col-lg-12 text-center">
							<div class="container">
								<input type="button" class="btn btn-primary btn-lg" value="Create Entry" disabled />
							</div>
						</div>
					</div>
				{/if}
			</div>
		</div>
	</div>

	<!-- {#if $connected}
		<p>
			Connected chain: chainId = {$chainId}
		</p>
		<p>
			chainData = {JSON.stringify($chainData)}
		</p>
		<p>
			Selected account: {$selectedAccount || 'not defined'}
		</p>

		<p>
			{checkAccount} Balance on {$chainData.name}:
			{#await balance}
				<span>waiting...</span>
			{:then value}
				<span>{value}</span>
			{/await}
			{$chainData.nativeCurrency.symbol}
		</p>

		<p>
			<button on:click={sendTip}
				>send 0.01 {$chainData.nativeCurrency.symbol} tip to {tipAddress} (author)</button
			>
		</p>
	{/if} -->
</main>

<style>
	main {
		text-align: center;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
