<script>
    export let color = {
        r: 255,
        g: 0,
        b: 0
    };

    export let styleContainer = undefined;
    export let styleSelector = undefined;

    import RangeSlider from './RangeSlider.svelte';
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();
    const zone1 = 100 / 6;
    const zone2 = zone1 * 2;
    const zone3 = zone1 * 3;
    const zone4 = zone1 * 4;
    const zone5 = zone1 * 5;

    let position = 0;

    $: update(position);

    function setColor(color, r, g, b) {
        color.r = r;
        color.g = g;
        color.b = b;
    }

    function updateByBoundary() {
        const x = Number(position.toString().slice(0, position.toString().indexOf('.') + 3));

        let result = false;

        switch (x) {
            case 16.66:
                setColor(color, 255, 255, 0);
                result = true;
                break;
            case 33.33:
                setColor(color, 0, 255, 0);
                result = true;

                break;
            case 50:
                setColor(color, 0, 255, 255);
                result = true;

                break;
            case 66.66:
                setColor(color, 0, 0, 255);
                result = true;

                break;
            case 83.33:
                setColor(color, 255, 0, 255);
                result = true;

                break;
            case 100:
                setColor(color, 255, 0, 0);
                result = true;

                break;
        }
    }

    function update() {
        if (!updateByBoundary()) {
            if (position >= 0 && position < zone1) {
                const percent = position * 6;
                color.g = Math.round(255 * percent / 100);
            } else if (position >= zone1 && position < zone2) {
                const percent = (position - zone1) * 6;
                color.r = 255 - Math.round(255 * percent / 100);
            } else if (position >= zone2 && position < zone3) {
                const percent = (position - zone2) * 6;
                color.b = Math.round(255 * percent / 100);
            } else if (position >= zone3 && position < zone4) {
                const percent = (position - zone3) * 6;
                color.g = 255 - Math.round(255 * percent / 100);
            } else if (position >= zone4 && position < zone5) {
                const percent = (position - zone4) * 6;
                color.r = Math.round(255 * percent / 100);
            } else if (position >= zone5) {
                const percent = (position - zone5) * 6;
                color.b = 255 - Math.round(255 * percent / 100);
            }
        }

        dispatch('colorChange', {
            color: color
        });
    }
</script>

<style>
    :global([styleTrack='hue']) {
        height: 15px;
        background: linear-gradient(to right, #ff0000, #ffff00, #00ff00, #00ffff, #0000ff, #ff00ff, #ff0000);
    }
</style>

<RangeSlider bind:position={position} styleContainer={styleContainer} styleTrack="hue" styleSelector={styleSelector} />