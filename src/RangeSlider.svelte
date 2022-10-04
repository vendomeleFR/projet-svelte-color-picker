<script>
    export let styleContainer = undefined;
    export let styleTrack = undefined;
    export let styleSelector = undefined;
    export let position = 0;
    export let background = 'none';

    let track;

    $: {
        position = (position < 0) ? 0 : position;
        position = (position > 100) ? 100 : position;
    }

    function onSelectorMouseMove(buttons, gapX) {
        if (buttons == 1) {
            position = position + gapX / track.clientWidth * 100;
        }
    }
</script>

<style>
    .track {
        width: 100%;
        margin: 0 auto;
        caret-color: transparent;
        position: relative;
    }

    .track-bg {
        background: var(--bg);
    }

    .selector-wrapper {
        position: absolute;
        padding: 4px;
        top: 50%;
        left: var(--left);
        transform: translate(-50%, -50%);
    }
</style>

<div {styleContainer}>
    <div bind:this={track} class="track" {styleTrack} class:track-bg={background != 'none'} style="--bg: {background}">
        <div class="selector-wrapper" style="--left: {position}%;" on:mousemove={(e) => onSelectorMouseMove(e.buttons, e.movementX)}>
            <div class="selector" {styleSelector} />
        </div>
    </div>
</div>