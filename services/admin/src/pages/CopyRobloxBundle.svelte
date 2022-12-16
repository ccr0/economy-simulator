<script lang="ts">
	import {link} from 'svelte-routing';
	import Main from "../components/templates/Main.svelte";
	import request from "../lib/request";
	let disabled = false;
	let errorMessage: string | undefined;
	import * as rank from "../stores/rank";
	let createdAssetId: number|undefined;
	let force = 'false';
	let didError = false;
</script>

<svelte:head>
	<title>Copy Roblox Bundle</title>
</svelte:head>

<Main>
	<div class="row">
		<div class="col-12">
			<h1>Copy Roblox Bundle</h1>
			{#if errorMessage}
				<p class="err">{errorMessage}</p>
			{/if}
			{#if createdAssetId !== undefined}
				<p>Link: <a href={`/catalog/${createdAssetId}/--`}>View on site</a></p>
				<p>Product: <a use:link href={`/admin/product/update?assetId=${createdAssetId}`}>Update Product</a></p>
			{/if}
		</div>
		<div class="col-12">
			<label for="url">Roblox URL</label>
			<input type="text" class="form-control" id="url" {disabled} />
		</div>

		<div class="col-6 mt-4">
			<button
				class="btn btn-success"
				{disabled}
				on:click={(e) => {
					e.preventDefault();
					if (disabled) {
						return;
					}
					disabled = true;
					createdAssetId = undefined;
					request
                    // @ts-ignore
						.post("/bundle/copy-from-roblox?bundleId=" + parseInt(document.getElementById("url").value.match(/[0-9]+/)[0], 10), {})
						.then((d) => {
							// window.location.href = `/catalog/${d.data.assetId}/--`;
							createdAssetId = d.data.assetId;
						})
						.catch((e) => {
							errorMessage = e.message;
							didError = true;
						})
						.finally(() => {
							disabled = false;
						});
				}}>Create Bundle</button
			>
		</div>
	</div>
</Main>

<style>
	p.err {
		color: red;
	}
	input#new-url {
		cursor: pointer;
	}
</style>
