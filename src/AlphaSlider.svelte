<script>
    export let styleContainer = undefined;
    export let styleSelector = undefined;
    export let color = undefined;

    import RangeSlider from './RangeSlider.svelte';
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    let position = 100;

    $: update(position);
    $: alphaBg = `linear-gradient(to right, rgba(${color.r}, ${color.g}, ${color.b}, 0), rgb(${color.r}, ${color.g}, ${color.b})), url('/assets/img/alpha.png')`;

    function update() {
        dispatch('change' , {
            alpha: position / 100
        })
    }
</script>

<style>
    :global([styleTrack='alpha']) {
        height: 15px;
        background-size: auto 100%;
    }
</style>

<RangeSlider bind:position={position} styleContainer={styleContainer} styleTrack="alpha" styleSelector={styleSelector} background={alphaBg} />