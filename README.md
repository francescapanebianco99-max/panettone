# Grandi lievitati fatti in casa

<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Componi il tuo Panettone</title>

<style>
body {
    font-family: 'Segoe UI', sans-serif;
    background: #f8f5f2;
    margin: 0;
    padding: 20px;
}

.container {
    max-width: 520px;
    margin: auto;
    background: #ffffff;
    padding: 25px;
    border-radius: 16px;
    box-shadow: 0 6px 20px rgba(0,0,0,0.08);
}

h1 {
    text-align: center;
    margin-bottom: 10px;
    font-size: 24px;
}

.subtitle {
    text-align: center;
    font-size: 14px;
    color: #777;
    margin-bottom: 20px;
}

label {
    margin-top: 18px;
    display: block;
    font-weight: 600;
    font-size: 14px;
}

select {
    width: 100%;
    padding: 10px;
    margin-top: 6px;
    border-radius: 10px;
    border: 1px solid #ddd;
    background: #fafafa;
    font-size: 14px;
}

button {
    width: 100%;
    padding: 12px;
    margin-top: 25px;
    border-radius: 10px;
    border: none;
    background: #6b3e26;
    color: white;
    font-size: 15px;
    font-weight: bold;
    cursor: pointer;
    transition: 0.2s;
}

button:hover {
    background: #8a5234;
}

.output {
    margin-top: 25px;
    white-space: pre-line;
    background: #fdfaf7;
    padding: 15px;
    border-radius: 10px;
    border: 1px solid #eee;
    font-size: 14px;
    line-height: 1.5;
}
</style>


//h1 {
    <div class="subtitle">Scopri ingredienti e composizione</div>
    //text-align: center;
}


</head>

<body>

<div class="container">
<h1>Componi il tuo lievitato</h1>

<label>Impasto</label>
<select id="impasto"></select>

<label>Sospensioni (puoi selezionarne più di una)</label>
<select id="sospensioni" multiple size="5"></select>

<label>Glassa</label>
<select id="glassa"></select>

<label>Decorazione</label>
<select id="decorazione"></select>

<button onclick="genera()">Genera ingredienti</button>

<div class="output" id="output"></div>
</div>

<script>

// DATABASE

const impasti = [
"Dolce tradizionale: Farina, lievito naturale, zucchero, uova, burro, miele, pasta di agrumi, vaniglia in bacche, malto, sale. Può contenere tracce di soia, senape e lupini",
"Integrale: Farina integrale, lievito naturale, zucchero, uova, burro, miele, pasta di agrumi, vaniglia in bacche, malto, sale. Può contenere tracce di soia, senape e lupini",
"Al caffè: Farina, lievito naturale, zucchero, uova, burro, latte, panna, caffè, amido, miele, pasta di caffè, vaniglia in bacche, malto, sale. Può contenere tracce di soia, senape, lupini, arachidi, mandorle, nocciole, pistacchi",
"Al cacao: Farina, lievito naturale, zucchero, uova, burro, miele, pasta di agrumi, cioccolato fondente, vaniglia in bacche, malto, sale. Può contenere tracce di soia, senape e lupini",
"Gianduia: Farina, lievito naturale, zucchero, uova, burro, miele, pasta di agrumi, crema di nocciola, vaniglia in bacche, malto, sale. Può contenere tracce di soia, senape e lupini",
"Salato: Farina, lievito naturale, zucchero, uova, olio extra vergine di oliva, strutto, malto, sale, origano. Può contenere tracce di soia, senape e lupini"
];

const sospensioni = [
"Canditi di arancia: scorze di arancia, zucchero",
"Canditi di limone: scorza di limone, zucchero",
"Canditi di albicocche: albicocche, zucchero",
"Canditi di kumquat: mandarini cinesi, zucchero",
"Noci sabbiate: noci, zucchero",
"Uvetta",
"Cioccolato bianco",
"Cioccolato al latte",
"Cioccolato fondente",
"Cioccolato al caramello",
"Cioccolato ruby",
"Gianduia",
"Marzapane: mandorle, albume, zucchero",
"Marzapane al pistacchio: mandorle, pasta di pistacchio, albume, zucchero",
"Marzapane al caffè: mandorle, pasta di caffè, albume, zucchero",
"Marzapane al lampone: mandorle, purea di lamponi, zucchero",
"Marzapane alla fragola: mandorle, purea di fragole, zucchero",
"Salame",
"Formaggio",
"Olive Taggiasche",
"Pomodori secchi"
];

const glasse = [
"Nessuna",
"Glassa tradizionale: mandorle, albume, zucchero, olio di semi, amido",
"Glassa alle nocciole: mandorle, nocciole, albume, zucchero, olio di semi, amido",
"Glassa al cioccolato: cioccolato fondente, olio di semi",
"Glassa al pistacchio: cioccolato bianco, burro di cacao, pasta pura di pistacchio, olio di semi",
"Glassa salata: mandorle, albume, parmigiano, olio di semi, amido"
];

const decorazioni = [
"Nessuna",
"Mandorle, zucchero al velo, granella di zucchero",
"Nocciole, zucchero al velo, granella di zucchero",
"Nocciole",
"Pistacchi",
"Semi misti"
];

// POPOLA MENU
function popola(selectId, lista) {
    const select = document.getElementById(selectId);
    lista.forEach(item => {
        let opt = document.createElement("option");
        opt.value = item;
        opt.text = item.split(":")[0];
        select.add(opt);
    });
}

// Esegui quando il DOM è completamente caricato
document.addEventListener('DOMContentLoaded', function() {
    popola("impasto", impasti);
    popola("sospensioni", sospensioni);
    popola("glassa", glasse);
    popola("decorazione", decorazioni);
});

// GENERA OUTPUT
function genera() {
    const impasto = document.getElementById("impasto").value;

    const sosp = Array.from(document.getElementById("sospensioni").selectedOptions)
        .map(o => o.value);

    const glassa = document.getElementById("glassa").value;
    const decorazione = document.getElementById("decorazione").value;

    let testo = "";

    testo += impasto + "\n\n";

    sosp.forEach(s => testo += s + "\n");

    if (sosp.length > 0) testo += "\n";

    if (glassa !== "Nessuna") testo += glassa + "\n";

    if (decorazione !== "Nessuna") testo += "\n" + decorazione;

    document.getElementById("output").innerText = testo;
}

</script>

</body>
</html>
