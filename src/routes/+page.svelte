<h1>Web apps are so powerful</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Building Cost Estimator</title>
    <style>
        body,*{font-family:Arial,sans-serif;margin:0;padding:0;box-sizing:border-box;}
        .estimator{width:100%;max-width:320px;margin:20px auto;padding:20px;background:#fff;border-radius:8px;box-shadow:0 4px 8px rgba(0,0,0,0.1);}
        h2{text-align:center;}
        label,button,select{display:block;width:100%;margin:5px 0;padding:10px;}
        button{background:#3498db;color:white;border:none;cursor:pointer;}
        #r{font-size:1.1em;text-align:center;margin-top:20px;}
    </style>
</head>
<body>
<div class="estimator">
    <h2>Building Cost Estimator</h2>
    <label>Area:</label>
    <select id="a">
        <option value="800">800 sqft</option>
        <option value="1600">1600 sqft</option>
        <option value="3200">3200 sqft</option>
    </select>
    <label>Floors:</label>
    <select id="f">
        <option value="1">1</option>
        <option value="2">2</option>
    </select>
    <label>Base:</label>
    <select id="b">
        <option value="s">Stem Wall</option>
        <option value="p">Pole (-25%)</option>
    </select>
    <label>Frame:</label>
    <select id="m">
        <option value="w">Wood</option>
        <option value="t">Steel (+15%)</option>
    </select>
    <button onclick="estimateCost()">Estimate</button>
    <div id="r"></div>
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
              o = b == 'p' ? 0.75 : m == 't' ? 1.15 : 1;

        document.getElementById('r').innerHTML = `<p>Total: $${(n * o * 1.05).toLocaleString()}</p><p>Per Sqft: $${(n * o * 1.05 / t).toFixed(2)}</p>`;
    }
</script>
</body>
</html>