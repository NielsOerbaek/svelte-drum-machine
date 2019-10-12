<script>
	import DrumMachine from "./DrumMachine.svelte"
	let width = 4;
	let height = 4;
	let bpm = 120;
	let useBothDirections = false
</script>

<style>
	.main {
		max-width: 1000px;
		margin: 0 auto;
	}
	h1 {
		color: purple;
	}
	#main {
		margin-left: 400px;
	}
	#left-sidebar {
		position: fixed;
		left: 0;
		top: 0;
		bottom: 0;
		width: 400px;
		padding: 25px;
	}
</style>

<div id="main">
	<div class="main">
		<DrumMachine
			width={width}
			height={height}
			bpm={bpm}
			useBothDirections={useBothDirections}
		/>
	</div>
</div>
<div id="left-sidebar" class="teal lighten-5">
	<h1>Svelte <br/>Drum <br/>Machine!</h1>
		<div class="settings">
			<h5>Settings:<h5>
			<label for="setting-height">Height:</label>
			<input type="number" bind:value={height} id="setting-height"/>
			<label for="setting-width">Width:</label>
			<input type="number" bind:value={width} id="setting-width"/>
			<label for="setting-bpm">BPM:</label>
			<input type="number" bind:value={bpm} id="setting-bpm"/>
			<div class="direction-selector">
				<div class="switch">
					<label>
						Directions:<br/>
						One
						<input type="checkbox" bind:checked={useBothDirections}>
						<span class="lever"></span>
						Two
					</label>
				</div>
			</div>
		</div>
	</div>
