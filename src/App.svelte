<script>
	import ColorPicker from './ColorPicker.svelte';
</script>

<main>
	<h1>Color Picker</h1>
	<ColorPicker />
</main>

<style>
	h1 {
		color: #ff3e00;
		font-size: 4em;
		font-weight: 100;
		text-align: center;
		margin-bottom: 15px;
	}

	:global([styleContainer='demo']) {
        width: 600px;
        padding: 10px 20px;
    }

	:global([styleTrack='demo']) {
        height: 10px;
        background: linear-gradient(to right, #a061b300, #a061b3), cyan;
    }

	:global([styleSelector='demo']) {
        width: 20px;
        height: 20px;
        background: #a061b3;
        border-radius: 100vh;
    }

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>