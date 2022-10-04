<script>
    import Chromatic from './Chromatic.svelte';
    import HueSlider from './HueSlider.svelte';
    import AlphaSlider from './AlphaSlider.svelte';

    const arr = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'];

    let refColor = {
        r: 255,
        g: 0,
        b: 0,
    };

    let preview = {
        r: 255,
        g: 0,
        b: 0,
        a: 1,
    };


    let previewBg = `linear-gradient(rgba(255, 0, 0, 1), rgba(255, 0, 0, 1)), url('/assets/img/alpha.png')`;
    let alpha = 1.0;
    let info = '#ff0000';

    function onChromaticColorChanged(color) {
        preview.r = color.r;
        preview.g = color.g;
        preview.b = color.b;

        updateInfo();
    }

    function onHueColorChanged(color) {
        refColor.r = color.r;
        refColor.g = color.g;
        refColor.b = color.b;

        updateInfo();
    }

    function onAlphaChange(value) {
        alpha = value;
        preview.a = alpha;

        updateInfo();
    }

    function updateInfo() {
        previewBg = `linear-gradient(rgba(${preview.r}, ${preview.g}, ${preview.b}, ${preview.a}), rgba(${preview.r}, ${preview.g}, ${preview.b}, ${preview.a})), url('../assets/img/alpha.png')`;

        let r = preview.r.toString(16);
        let g = preview.g.toString(16);
        let b = preview.b.toString(16);

        r = arr.indexOf(r) != -1 ? `0${r}` : r;
        g = arr.indexOf(g) != -1 ? `0${g}` : g;
        b = arr.indexOf(b) != -1 ? `0${b}` : b;

        info = `#${r}${g}${b}`;

        if (alpha < 1) {
            let a = Math.round(alpha * 255).toString(16);
            a = arr.indexOf(a) != -1 ? `0${a}` : a;
            info += a;
        }
    }
</script>

<style>
    .container {
        display: flex;
        flex-direction: column;
        gap: 8px;
        width: fit-content;
        margin: 0 auto;
    }

    .preview-sliders {
        display: flex;
        flex-direction: row;
    }

    .preview i {
        display: block;
        width: 60px;
        height: 60px;
        border-radius: 100vh;
        background: var(--bg);
    }

    .sliders {
        display: flex;
        flex-direction: column;
        justify-content: space-around;
    }

    :global([styleContainer='sliders']) {
        width: 400px;
        padding: 10px 20px;
    }

    :global([styleSelector='sliders']) {
        width: 16px;
        height: 16px;
        background: white;
        box-shadow: 0px 0px 4px 1px #00000099;
        border-radius: 100vh;
    }

    .info span {
        display: block;
        width: 100%;
        text-align: center;
    }
</style>

<div class="container">
    <div class="chrome">
        <Chromatic height="300px" refColor={refColor} on:colorChange={(e) => onChromaticColorChanged(e.detail.color)} />
    </div>
    <div class="preview-sliders">
        <div class="preview">
            <i style="--bg: {previewBg};" />
        </div>
        <div class="sliders">
            <HueSlider styleContainer="sliders" styleSelector="sliders" on:colorChange={(e) => onHueColorChanged(e.detail.color)} />
            <AlphaSlider color={refColor} styleContainer="sliders" styleSelector="sliders" on:change={(e) => onAlphaChange(e.detail.alpha)} />
        </div>
    </div>
    <div class="info">
        <span>{info}</span>
    </div>
</div>
