# Fitness-lite-Pace-Calculator-2.0
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CrossFit Engine Zones v5 - Validated</title>
    <style>
        body { font-family: sans-serif; background-color: #121212; color: #e0e0e0; padding: 15px; margin: 0; }
        .box { max-width: 500px; margin: 0 auto; }
        h1 { text-align: center; color: #ff9800; font-size: 22px; margin-bottom: 5px; }
        .subtitle { text-align: center; color: #888; font-size: 13px; margin-bottom: 20px; }
        h2 { color: #ff9800; border-bottom: 1px solid #333; padding-bottom: 5px; font-size: 16px; margin-top: 25px; }
        .card { background: #1e1e1e; padding: 12px; border-radius: 8px; margin-bottom: 10px; border-left: 4px solid #ff9800; }
        label { display: block; font-size: 12px; color: #aaa; margin-bottom: 5px; font-weight: bold; }
        .flex { display: flex; gap: 10px; }
        input { flex: 1; padding: 10px; border-radius: 5px; border: 1px solid #444; background: #2a2a2a; color: #fff; text-align: center; font-size: 16px; }
        table { width: 100%; border-collapse: collapse; margin-top: 5px; background: #1e1e1e; border-radius: 6px; overflow: hidden; }
        th, td { padding: 10px; text-align: center; border-bottom: 1px solid #2a2a2a; font-size: 13px; }
        th { background: #2a2a2a; color: #ff9800; }
        .vo2 { color: #f44336; font-weight: bold; }
        .soglia { color: #ff9800; font-weight: bold; }
        .z23 { color: #2196f3; font-weight: bold; }
        .z2 { color: #4caf50; font-weight: bold; }
    </style>
</head>
<body>
<div class="box">
    <h1>CrossFit Target Zones App</h1>
    <div class="subtitle">Validazione Scientifica PubMed & Formule Concept2</div>
    
    <div class="card"><label>1. CORSA 5000m (Tempo)</label><div class="flex"><input type="number" id="r_m" value="22" placeholder="Min"><input type="number" id="r_s" value="30" placeholder="Sec"></div></div>
    <div class="card"><label>2. ROWER 10' TEST (Calorie totali display)</label><input type="number" id="row_cal" value="160"></div>
    <div class="card"><label>3. ASSAULT / ECHO BIKE 10' (Calorie totali display)</label><input type="number" id="ab_cal" value="150"></div>
    <div class="card"><label>4. BIKE ERG 10' TEST (Calorie totali display)</label><input type="number" id="be_cal" value="180"></div>

    <h2>1. Corsa</h2>
    <table>
        <thead><tr><th>Zona</th><th>Passo (/km)</th></tr></thead>
        <tbody>
            <tr class="vo2"><td>VO2max (Pace Test)</td><td id="r1">-</td></tr>
            <tr class="soglia"><td>Soglia / Critical Speed</td><td id="r2">-</td></tr>
            <tr class="z23"><td>Finestra Zona 2-3</td><td id="r23">-</td></tr>
            <tr class="z2"><td>Zona 2 (Aerobico)</td><td id="r3">-</td></tr>
        </tbody>
    </table>

    <h2>2. Rower (Concept2)</h2>
    <table>
        <thead><tr><th>Zona</th><th>Ritmo /500m</th><th>Cal / min</th></tr></thead>
        <tbody>
            <tr class="vo2"><td>VO2max (Pace Test 10')</td><td id="ro1">-</td><td id="ro2">-</td></tr>
            <tr class="soglia"><td>Soglia / Critical Power</td><td id="ro3">-</td><td id="ro4">-</td></tr>
            <tr class="z23"><td>Finestra Zona 2-3</td><td id="ro_z23_p">-</td><td id="ro_z23_c">-</td></tr>
            <tr class="z2"><td>Zona 2 (Aerobico)</td><td id="ro5">-</td><td id="ro6">-</td></tr>
        </tbody>
    </table>

    <h2>3. Assault / Echo Bike</h2>
    <table>
        <thead><tr><th>Zona</th><th>Cal / min</th><th>Watt</th></tr></thead>
        <tbody>
            <tr class="vo2"><td>VO2max (Pace Test 10')</td><td id="a1">-</td><td id="aw1">-</td></tr>
            <tr class="soglia"><td>Soglia / Critical Power</td><td id="a2">-</td><td id="aw2">-</td></tr>
            <tr class="z23"><td>Finestra Zona 2-3</td><td id="a_z23_c">-</td><td id="a_z23_w">-</td></tr>
            <tr class="z2"><td>Zona 2 (Aerobico)</td><td id="a3">-</td><td id="aw3">-</td></tr>
        </tbody>
    </table>

    <h2>4. BikeErg (Concept2)</h2>
    <table>
        <thead><tr><th>Zona</th><th>Ritmo /1000m</th><th>Cal / min</th></tr></thead>
        <tbody>
            <tr class="vo2"><td>VO2max (Pace Test 10')</td><td id="b1">-</td><td id="b2">-</td></tr>
            <tr class="soglia"><td>Soglia / Critical Power</td><td id="b3">-</td><td id="b4">-</td></tr>
            <tr class="z23"><td>Finestra Zona 2-3</td><td id="be_z23_p">-</td><td id="be_z23_c">-</td></tr>
            <tr class="z2"><td>Zona 2 (Aerobico)</td><td id="b5">-</td><td id="b6">-</td></tr>
        </tbody>
    </table>
</div>

<script>
    function t(sec) { 
        if (sec <= 0 || isNaN(sec) || !isFinite(sec)) return "-";
        let m = Math.floor(sec / 60), s = Math.round(sec % 60); 
        return m + ":" + (s < 10 ? "0" : "") + s; 
    }
    
    // Converte i Watt reali del Rower nel passo sui 500m
    function wattsToPaceRow(w) {
        if (w <= 0) return 0;
        return 500 * Math.pow(2.8 / w, 1/3);
    }

    // Converte i Watt reali della BikeErg nel passo sui 1000m
    function wattsToPaceBikeErg(w) {
        if (w <= 0) return 0;
        return 1000 * Math.pow(2.8 / w, 1/3);
    }

    // Isola i Watt reali partendo dalle Calorie/ora totali (sottraendo la costante metabolica di 300)
    function calMinToWatts(calMin) {
        let calOra = calMin * 60;
        if (calOra <= 300) return 0;
        return (calOra - 300) / 3.4417;
    }

    // Riconverte i Watt corretti nelle corrispondenti Calorie al minuto per il display
    function wattsToCalMin(w) {
        if (w <= 0) return 0;
        return ((w * 3.4417) + 300) / 60;
    }

    function abCalToWatts(calMin) {
        let calOra = calMin * 60;
        if (calOra <= 0) return 0;
        return Math.round(Math.pow(calOra / 15.625, 1 / 0.672));
    }

    function calc() {
        let rm = parseFloat(document.getElementById('r_m').value)||0, 
            rs = parseFloat(document.getElementById('r_s').value)||0, 
            rowCal = parseFloat(document.getElementById('row_cal').value)||0, 
            abCal = parseFloat(document.getElementById('ab_cal').value)||0, 
            beCal = parseFloat(document.getElementById('be_cal').value)||0;
        
        // 1. CORSA
        let runSec = (rm * 60) + rs;
        if (runSec > 0) { 
            let testPace = runSec / 5; // s/km
            let sogliaPace = testPace * 1.05; // La soglia è circa il 5% più lenta del test massimale sui 5k
            
            document.getElementById('r1').innerText = t(testPace) + " /km";
            document.getElementById('r2').innerText = t(sogliaPace) + " /km"; 
            document.getElementById('r3').innerText = t(sogliaPace * 1.25) + " /km"; // Zona 2 (75-80% della velocità di soglia, quindi più lenta)
            document.getElementById('r23').innerText = t(sogliaPace * 1.20) + " - " + t(sogliaPace * 1.10) + " /km"; // Finestra Z2-Z3
        }
        
        // 2. ROWER
        if (rowCal > 0) {
            let testCalMin = rowCal / 10;
            let testWatts = calMinToWatts(testCalMin);
            
            // Fisiologia: la soglia critica è stimata al 93% dei watt massimali espressi nei 10'
            let sogliaWatts = testWatts * 0.93;
            let z2Watts = sogliaWatts * 0.70;
            let z23WattsMin = sogliaWatts * 0.75;
            let z23WattsMax = sogliaWatts * 0.85;

            document.getElementById('ro1').innerText = t(wattsToPaceRow(testWatts)) + " /500m"; 
            document.getElementById('ro2').innerText = testCalMin.toFixed(1) + " cal/m";

            document.getElementById('ro3').innerText = t(wattsToPaceRow(sogliaWatts)) + " /500m"; 
            document.getElementById('ro4').innerText = wattsToCalMin(sogliaWatts).toFixed(1) + " cal/m";
            
            document.getElementById('ro5').innerText = t(wattsToPaceRow(z2Watts)) + " /500m"; 
            document.getElementById('ro6').innerText = wattsToCalMin(z2Watts).toFixed(1) + " cal/m";

            document.getElementById('ro_z23_p').innerText = t(wattsToPaceRow(z23WattsMin)) + " - " + t(wattsToPaceRow(z23WattsMax));
            document.getElementById('ro_z23_c').innerText = wattsToCalMin(z23WattsMin).toFixed(1) + " - " + wattsToCalMin(z23WattsMax).toFixed(1) + " cal/m";
        }

        // 3. ASSAULT BIKE
        if (abCal > 0) { 
            let testCalMin = abCal / 10;
            let testWatts = abCalToWatts(testCalMin);
            
            let sogliaCalMin = testCalMin * 0.92;
            let z2CalMin = sogliaCalMin * 0.70;
            let z23CalMinMin = sogliaCalMin * 0.75;
            let z23CalMinMax = sogliaCalMin * 0.85;

            document.getElementById('a1').innerText = testCalMin.toFixed(1) + " cal/m"; 
            document.getElementById('aw1').innerText = testWatts + " W";

            document.getElementById('a2').innerText = sogliaCalMin.toFixed(1) + " cal/m"; 
            document.getElementById('aw2').innerText = abCalToWatts(sogliaCalMin) + " W";

            document.getElementById('a3').innerText = z2CalMin.toFixed(1) + " cal/m"; 
            document.getElementById('aw3').innerText = abCalToWatts(z2CalMin) + " W";

            document.getElementById('a_z23_c').innerText = z23CalMinMin.toFixed(1) + " - " + z23CalMinMax.toFixed(1) + " cal/m";
            document.getElementById('a_z23_w').innerText = abCalToWatts(z23CalMinMin) + " - " + abCalToWatts(z23CalMinMax) + " W";
        }

        // 4. BIKE ERG
        if (beCal > 0) {
            let testCalMin = beCal / 10;
            let testWatts = calMinToWatts(testCalMin);
            
            let sogliaWatts = testWatts * 0.93;
            let z2Watts = sogliaWatts * 0.70;
            let z23WattsMin = sogliaWatts * 0.75;
            let z23WattsMax = sogliaWatts * 0.85;

            document.getElementById('b1').innerText = t(wattsToPaceBikeErg(testWatts)) + " /1000m"; 
            document.getElementById('b2').innerText = testCalMin.toFixed(1) + " cal/m";

            document.getElementById('b3').innerText = t(wattsToPaceBikeErg(sogliaWatts)) + " /1000m"; 
            document.getElementById('b4').innerText = wattsToCalMin(sogliaWatts).toFixed(1) + " cal/m";
            
            document.getElementById('b5').innerText = t(wattsToPaceBikeErg(z2Watts)) + " /1000m"; 
            document.getElementById('b6').innerText = wattsToCalMin(z2Watts).toFixed(1) + " cal/m";

            document.getElementById('be_z23_p').innerText = t(wattsToPaceBikeErg(z23WattsMin)) + " - " + t(wattsToPaceBikeErg(z23WattsMax));
            document.getElementById('be_z23_c').innerText = wattsToCalMin(z23WattsMin).toFixed(1) + " - " + wattsToCalMin(z23WattsMax).toFixed(1) + " cal/m";
        }
    }
    document.querySelectorAll('input').forEach(i => i.addEventListener('input', calc)); 
    calc();
</script>
</body>
</html>
