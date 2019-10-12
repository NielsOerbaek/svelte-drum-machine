<script>
	import DrumMachine from "./DrumMachine.svelte"
	let width = 4;
	let height = 4;
	let bpm = 120;
	let useBothDirections = false
	let play = true
	let goRight = true
	let goDown = false
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
			{width}
			{height}
			{bpm}
			{play}
			{goRight}
			{goDown}
		/>
	</div>
</div>
<div id="left-sidebar" class="teal lighten-5">
	<h1>Svelte <br/>Drum <br/>Machine!</h1>
		<div class="settings">
			<h5>Settings:<h5>
			<a class={"waves-effect waves-light btn" + (play ? " disabled" : "")} on:click={() => play = true}><i class="material-icons left">play_arrow</i>Play</a>
			<a class={"waves-effect waves-light btn" + (!play ? " disabled" : "")} on:click={() => play = false}><i class="material-icons left">pause</i>Pause</a>
			<div class="direction-selector">
				<h6>Directions:</h6>
				<div class="switch">
					<label>
						<input type="checkbox" checked="checked" bind:checked={goRight} />
						<span>Go Right!</span>
					</label>
					<label>
						<input type="checkbox" bind:checked={goDown} />
						<span>Go Down!</span>
					</label>
				</div>
			</div>
			<label for="setting-height">Height:</label>
			<input type="number" bind:value={height} id="setting-height"/>
			<label for="setting-width">Width:</label>
			<input type="number" bind:value={width} id="setting-width"/>
			<label for="setting-bpm">BPM:</label>
			<input type="number" bind:value={bpm} id="setting-bpm"/>
		</div>
	</div>
