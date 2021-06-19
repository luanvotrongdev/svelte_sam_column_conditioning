<script lang="ts">
	import FileSelector from "./FileSelector.svelte";
	// import Version from "./Version.svelte";
	import * as XLSXLogic from "./XLSXLogic";
	import XLSX from 'xlsx';
	import TableRenderer from './TableRenderer.svelte'

	export let name: string;
	var inputFile: FileList;
	var jsonTableRenderer: TableRenderer;
	var textField: HTMLParagraphElement;

	function onMatchBtnPressed() {
		switch (XLSXLogic.validateInput(inputFile)) {
			case XLSXLogic.ErrorCode.MISSING_INPUT:
				textField.innerText = "missing input file!";
				break;
			case XLSXLogic.ErrorCode.UNKNOWN:
				textField.innerText = "Unknown error!";
				break;
			case XLSXLogic.ErrorCode.NONE:
				textField.innerText = "";
				XLSXLogic.readXLSXFile(inputFile[0]).then(workbook=>{
						var sheet : XLSX.WorkSheet = workbook.Sheets[workbook.SheetNames[0]];
						var json = XLSXLogic.renderXLSXSheetToJson(sheet);
						jsonTableRenderer.setArray(json);
				})
				break;
		}
	}
</script>


<svelte:head>
	<title>sam's column conditioning</title>
	<meta name="robots" content="noindex nofollow" />
	<html lang="en" />
</svelte:head>

<main>
	<h1>Hello <span class="name">{name}</span>!</h1>
	<p><span class="name">SAM</span>'s column conditioning.</p>

	<FileSelector bind:files={inputFile}/>
	<button on:click={onMatchBtnPressed}>MATCH!</button>

	<p bind:this={textField} />
	<!-- <Version version="v1.0.0"></Version> -->

	<TableRenderer bind:this={jsonTableRenderer}></TableRenderer>
</main>


<style>
	main {
		text-align: CENTER;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #000000;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	.name {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 1em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
