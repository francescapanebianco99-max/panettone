
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body {
    font-family: 'Segoe UI', sans-serif;
    background: #f8f5f2;
    margin: 0;
    padding: 20px;
}

/* CONTAINER */
.container {
    max-width: 520px;
    margin: auto;
    background: #ffffff;
    padding: 25px;
    border-radius: 16px;
    box-shadow: 0 6px 20px rgba(0,0,0,0.08);
}

/* LOGO */
.logo {
    text-align: center;
    margin-bottom: 15px;
}

.logo img {
    width: 30%;
    max-width: 120px;
    height: auto;
    opacity: 0.9;
}

/* TITOLI */
h2 {
    text-align: center;
    margin-bottom: 15px;
    font-size: 24px;
}
h1 {
    text-align: center;
    margin-bottom: 15px;
    font-size: 24px;
    display: none
}
/* FORM */
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
    margin-bottom: 5px;
    border-radius: 10px;
    border: 1px solid #ddd;
    background: #fafafa;
    font-size: 14px;
}

/* MULTISELECT */
select[multiple] {
    height: auto;
}

/* BUTTON */
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
}

button:hover {
    background: #8a5234;
}

/* OUTPUT */
.output {
    margin-top: 25px;
    background: #fdfaf7;
    padding: 15px;
    border-radius: 10px;
    border: 1px solid #eee;
    font-size: 14px;
    line-height: 1.5;
}

/* INTRO */
.intro {
    text-align: center;
    font-size: 13px;
    color: #6b3e26;
    line-height: 1.5;
    margin-bottom: 10px;
    font-weight: bold;
}

.impasto-title {
    font-weight: 600;
}

/* 📱 MOBILE */
@media (max-width: 600px) {
    body {
        padding: 10px;
    }

    .container {
        padding: 15px;
        border-radius: 12px;
    }

    h2 {
        font-size: 20px;
    }

    label {
        font-size: 13px;
    }

    select, button {
        font-size: 14px;
        padding: 10px;
    }

    .output {
        font-size: 13px;
    }
}

/* 💻 DESKTOP GRANDE */
@media (min-width: 900px) {
    .container {
        max-width: 600px;
    }
}
</style>
</head>

<body>

<div class="container">

    <div class="logo">
        <img src="logo.png" alt="logo">
    </div>

    <h2>Componi il tuo lievitato</h2>

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
"Impasto tradizionale: Farina, lievito naturale, zucchero, uova, burro, miele, pasta di agrumi, vaniglia in bacche, malto, sale. Può contenere tracce di soia, senape e lupini",
"Impasto Integrale: Farina integrale, lievito naturale, zucchero, uova, burro, miele, pasta di agrumi, vaniglia in bacche, malto, sale. Può contenere tracce di soia, senape e lupini",
"Impasto Salato: Farina, lievito naturale, zucchero, uova, olio extra vergine di oliva, strutto, malto, sale, origano. Può contenere tracce di soia, senape e lupini"
];

const sospensioni = [
"Canditi di arancia: scorze di arancia, zucchero",
"Canditi di limone: scorza di limone, zucchero",
"Uvetta",
"Cioccolato fondente",
"Gianduia",
"Marzapane: mandorle, albume, zucchero",
"Salame",
"Formaggio",
"Olive Taggiasche",
"Pomodori secchi"
];

const glasse = [
"Nessuna",
"Glassa tradizionale: mandorle, albume, zucchero, olio di semi, amido",
"Glassa al cioccolato: cioccolato fondente, olio di semi"
];

const decorazioni = [
"Nessuna",
"Mandorle, zucchero al velo, granella di zucchero",
"Nocciole",
"Pistacchi"
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

    testo += `
    <div class="intro">
        Il prodotto NON contiene conservanti<br>
        Il lievito naturale ne preserva la qualità per non oltre due settimane <br> dalla data di produzione
    </div>
    `;

    let partiImpasto = impasto.split(":");
    testo += `<span class="impasto-title">${partiImpasto[0]}:</span> ${partiImpasto[1]}<br><br>`;

    sosp.forEach(s => testo += s + "<br>");

    if (sosp.length > 0) testo += "<br>";

    if (glassa !== "Nessuna") testo += glassa + "<br>";

    if (decorazione !== "Nessuna") testo += "<br>" + decorazione;

    document.getElementById("output").innerHTML = testo;
}

</script>

</body>
</html>
