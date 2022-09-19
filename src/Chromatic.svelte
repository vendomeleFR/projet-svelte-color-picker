<script>
    export let height = '100px';
    export let refColor = undefined;
    export let position = {
        x: 100,
        y: 0
    };

    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    let area;
    let chromeColor;
    let selectedColor = {
        r: refColor.r,
        g: refColor.g,
        b: refColor.b
    }
    
    $: {
        chromeColor = (refColor) ? `rgb(${refColor.r}, ${refColor.g}, ${refColor.b})` : 'none';
        update();
    }

    function update() {
        if (selectedColor) {
            const color = {
                r: 255 - Math.round(position.x * (255 - refColor.r) / 100),
                g: 255 - Math.round(position.x * (255 - refColor.g) / 100),
                b: 255 - Math.round(position.x * (255 - refColor.b) / 100),
            };

            selectedColor.r = color.r - Math.round(position.y * color.r / 100);
            selectedColor.g = color.g - Math.round(position.y * color.g / 100);
            selectedColor.b = color.b - Math.round(position.y * color.b / 100);

            dispatch('colorChange', {
                color: selectedColor
            })
        }
    }

    function onAreaClicked(x, y) {
        position.x = x / area.clientWidth * 100;
        position.y = y / area.clientHeight * 100;

        update();
    }

    function onAreaMoved(e) {
        e.preventDefault();

        if (e.buttons == 1) {
            let valueX = position.x + e.movementX / area.clientWidth * 100;
            let valueY = position.y + e.movementY / area.clientHeight * 100;

            valueX = (valueX < 0) ? 0 : (valueX > 100) ? 100 : valueX;
            valueX = (valueX < 0.5) ? 0 : (valueX > 99.5) ? 100 : valueX;
            valueY = (valueY < 0) ? 0 : (valueY > 100) ? 100 : valueY;
            valueY = (valueY < 0.5) ? 0 : (valueY > 99.5) ? 100 : valueY;

            position.x = valueX;
            position.y = valueY;

            update();
        }
    }
</script>

<style>
    .container {
        width: 100%;
        height: var(--height);
        border: thin solid #beb5b5;
        background: var(--color);
        position: relative;
        caret-color: transparent;
        overflow: hidden;
    }

    .container div {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
    }

    .white {
		background: linear-gradient(to right, #fff, #fff0);
	}

    .black {
		background: linear-gradient(to top, #000, #0000);
	}

    .container i {
        position: absolute;
        display: block;
        width: 10px;
        height: 10px;
        border-radius: 100vh;
        box-shadow: inset 0px 0px 3px 1px white, 0px 0px 4px 1px black;
        left: var(--left);
        top: var(--top);
        transform: translate(-50%, -50%);
    }

</style>

<div class="container" style="--height: {height}; --color: {chromeColor};">
    <div class="white" />
    <div class="black" />
    <i style="--left: {position.x}%; --top: {position.y}%;" />
    <div bind:this={area} on:mousedown={(e) => onAreaClicked(e.layerX, e.layerY)} on:mousemove={(e) => onAreaMoved(e)} />
</div>