<script>
    import { onMount } from 'svelte';

    let area = '800';
    let floors = '1';
    let foundation = 's';
    let frame = 'w';
    let result;

    const areas = [
        { value: '800', label: '800 sqft' },
        { value: '1600', label: '1600 sqft' },
        { value: '3200', label: '3200 sqft' }
    ];

    const foundations = [
        { value: 's', label: 'Stem Wall' },
        { value: 'p', label: 'Pole Barn / Pole Construction (-25%)' }
    ];

    const frames = [
        { value: 'w', label: 'Wood' },
        { value: 't', label: 'Steel (+15%)' }
    ];

    function estimateCost() {
        const t = floors * area;
        const s = [1, 0.9, 0.75];
        const d = s[Object.keys(s).find(k => area >= k * 800)] || 1;
        const e = 70 * d;
        const g = {s: 10, p: 7.5};
        const h = {w: 20, t: 23};
        const i = 14, j = 22, k = 28;
        const l = floors > 1 ? area * 0.15 : 0;
        const n = t * e + l + area * (g[foundation] + h[frame] + i + j + k);
        const o = foundation === 'p' ? 0.75 : frame === 't' ? 1.15 : 1;

        const totalCost = n * o * 1.05;
        result = `<p>Total: $${totalCost.toLocaleString()}</p><p>Per Sqft: $${(totalCost / t).toFixed(2)}</p>`;
    }

    // For initial calculation when component mounts
    onMount(() => {
        estimateCost();
    });
</script>

<style>
    :global(body) {
        font-family: 'Roboto', sans-serif;
        background: linear-gradient(to right, #004e92, #000428);
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        margin: 0;
        color: #fff;
    }

    .estimator {
        background: rgba(255, 255, 255, 0.1);
        padding: 2rem;
        border-radius: 15px;
        box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
        width: 350px;
    }

    h2 {
        color: #FFD700;
        margin-bottom: 1.5rem;
        text-align: center;
    }

    .input-group {
        margin-bottom: 1rem;
    }

    label {
        display: block;
        margin-bottom: 0.5rem;
        color: #D3D3D3;
    }

    select, button {
        width: 100%;
        padding: 10px;
        border: none;
        border-radius: 5px;
        background: rgba(255, 255, 255, 0.2);
        color: white;
        font-size: 16px;
    }

    select {
        appearance: none;
        background-image: url('data:image/svg+xml;utf8,<svg fill="%23fff" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg"><path d="M7 10l5 5 5-5z"/><path d="M0 0h24v24H0z" fill="none"/></svg>');
        background-repeat: no-repeat;
        background-position-x: 98%;
        background-position-y: 50%;
    }

    option {
        background: #333;
    }

    button {
        background: #FFD700;
        color: #333;
        cursor: pointer;
        transition: background 0.3s;
    }

    button:hover {
        background: #FFC400;
    }

    #result {
        text-align: center;
        margin-top: 20px;
        font-size: 18px;
        color: #FFD700;
    }
</style>

<div class="estimator">
    <h2>BearLake.Build Estimator</h2>
    
    <div class="input-group">
        <label for="area">Area:</label>
        <select bind:value={area} id="area">
            {#each areas as option}
                <option value={option.value}>{option.label}</option>
            {/each}
        </select>
    </div>
    
    <div class="input-group">
        <label for="floors">Floors:</label>
        <select bind:value={floors} id="floors">
            <option value="1">1</option>
            <option value="2">2</option>
        </select>
    </div>
    
    <div class="input-group">
        <label for="foundation">Foundation:</label>
        <select bind:value={foundation} id="foundation">
            {#each foundations as option}
                <option value={option.value}>{option.label}</option>
            {/each}
        </select>
    </div>
    
    <div class="input-group">
        <label for="frame">Frame:</label>
        <select bind:value={frame} id="frame">
            {#each frames as option}
                <option value={option.value}>{option.label}</option>
            {/each}
        </select>
    </div>
    
    <button on:click={estimateCost}>Get Estimate</button>
    
    <div id="result" bind:innerHTML={result}></div>
</div>