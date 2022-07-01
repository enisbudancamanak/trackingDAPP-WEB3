<script>
	import Nav from '../components/Nav.svelte';
	import { onMount } from 'svelte';
	import jQuery from 'jquery';

	import {
		defaultEvmStores,
		web3,
		selectedAccount,
		connected,
		chainId,
		chainData
	} from 'svelte-web3';

	export let address = '0x6fae2E9C348025F39Af1C2E4Ecba3749c3BED9fE';

	let allJsonEntries = [];

	onMount(async () => {
		defaultEvmStores.disconnect();
		await defaultEvmStores.setBrowserProvider('http://localhost:7545').then(async () => {
			var currentBlock = await $web3.eth.getBlockNumber();
			var n = await $web3.eth.getTransactionCount(address, 7);

			for (var i = currentBlock; i >= 0; i--) {
				try {
					var block = await $web3.eth.getBlock(i, true);
					if (block && block.transactions) {
						{
							block.transactions.forEach(function (e) {
								if (address == e.to) {
									if (e.from != e.to) {
										if (isJsonObject(hexToUtf8(e.input).replace('0x', ''))) {
											allJsonEntries.push(JSON.parse(hexToUtf8(e.input).replace('0x', '')));
											// console.log(i, e.from, e.to, e.value.toString(10), hexToUtf8(e.input));
										}
									}
								}
							});
						}
					}
				} catch (error) {}
			}
		});

		buildTable(allJsonEntries);
	});

	// @ts-ignore
	function isJsonObject(strData) {
		try {
			JSON.parse(strData);
		} catch (e) {
			return false;
		}
		return true;
	}

	// @ts-ignore
	function hexToUtf8(s) {
		return decodeURIComponent(
			s
				.replace(/\s+/g, '') // remove spaces
				.replace(/[0-9a-f]{2}/g, '%$&') // add '%' before each 2 characters
		);
	}
	function buildTable(data) {
		jQuery('#searchInput').on('keyup', function () {
			var value = jQuery(this).val().toLowerCase();
			jQuery('.table tbody tr').filter(function () {
				jQuery(this).toggle(jQuery(this).text().toLowerCase().indexOf(value) > -1);
			});
		});

		var table = document.getElementById('tbody');

		for (var i = 0; i < data.length; i++) {
			var row = `<tr>
							<td>${data[i].id}</td>
							<td>${data[i].fishName}</td>
							<td>${data[i].catched.length > 0 ? data[i].catched : 'unknown'}</td>
							<td>${data[i].species}</td>
							<td>${data[i].gender}</td>
							<td>${data[i].healthNote}</td>
						</tr>`;
			table.innerHTML += row;
		}
	}
</script>

<svelte:head>
	<title>Fish Entry System using Blockchain</title>
</svelte:head>

<Nav />

<main>
	<div
		style="display: flex; flex-direction: column; justify-content: center; height: 100vh;"
		class="container"
	>
		<div class="right">
			<div class="container">
				<div class="row no-gutters">
					<div class="col-lg-12 text-center">
						<div class="card-body">
							<h4 class="card-title">Find your Fish in the Blockchain</h4>
						</div>
					</div>
				</div>
			</div>

			<div style="width: 10%; margin-left: 4%" class="form-group">
				<input class="form-control" id="searchInput" type="text" placeholder="filter id" />
			</div>
			<table class="table container">
				<thead>
					<tr>
						<th scope="col">id</th>
						<th scope="col">fishName</th>
						<th scope="col">catched</th>
						<th scope="col">species</th>
						<th scope="col">gender</th>
						<th scope="col">healthNote</th>
					</tr>
				</thead>
				<tbody id="tbody" />
			</table>
		</div>
	</div>
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
