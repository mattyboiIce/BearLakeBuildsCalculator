<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BearLake.Build Cost Estimator</title>
<style>
    body {
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
</head>
<body>
<div class="estimator">
    <h2>BearLake.Build Estimator</h2>
    <div class="input-group">
        <label for="a">Area:</label>
        <select id="a">
            <option value="800">800 sqft</option>
            <option value="1600">1600 sqft</option>
            <option value="3200">3200 sqft</option>
        </select>
    </div>
    <div class="input-group">
        <label for="f">Floors:</label>
        <select id="f">
            <option value="1">1</option>
            <option value="2">2</option>
        </select>
    </div>
    <div class="input-group">
        <label for="b">Foundation:</label>
        <select id="b">
            <option value="s">Stem Wall</option>
            <option value="p">Pole Barn / Pole Construction (-25%)</option>
        </select>
    </div>
    <div class="input-group">
        <label for="m">Frame:</label>
        <select id="m">
            <option value="w">Wood</option>
            <option value="t">Steel (+15%)</option>
        </select>
    </div>
    <button onclick="estimateCost()">Get Estimate</button>
    <div id="result"></div>
</div>
<script>
function estimateCost() {
    const a = +document.getElementById('a').value,
          f = +document.getElementById('f').value,
          b = document.getElementById('b').value,
          m = document.getElementById('m').value,
          t = f * a,
          s = [1, 0.9, 0.75],
          d = s[Object.keys(s).find(k => a >= k * 800)] || 1,
          e = 70 * d,
          g = {s: 10, p: 7.5},
          h = {w: 20, t: 23},
          i = 14, j = 22, k = 28,
          l = f > 1 ? a * 0.15 : 0,
          n = t * e + l + a * (g[b] + h[m] + i + j + k),
          o = b === 'p' ? 0.75 : m === 't' ? 1.15 : 1,
          totalCost = n * o * 1.05;

    document.getElementById('result').innerHTML = 
        `<p>Total: $${totalCost.toLocaleString()}</p><p>Per Sqft: $${(totalCost / t).toFixed(2)}</p>`;
}
</script>
</body>
</html>