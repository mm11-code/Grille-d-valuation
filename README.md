<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jeu de Placement des Critères</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .grid-container { display: flex; justify-content: center; margin-top: 20px; }
        .grid { display: grid; grid-template-columns: 200px repeat(4, 200px); gap: 5px; }
        .cell, .header { width: 200px; height: 100px; border: 1px solid black; display: flex; align-items: center; justify-content: center; background-color: #f0f0f0; }
        .header { font-weight: bold; background-color: #ddd; }
        .criteria-box { margin: 10px; padding: 10px; background: lightblue; display: inline-block; cursor: pointer; }
    </style>
</head>
<body>
    <h1>Placez les critères dans la grille</h1>
    <div id="criteria-container"></div>
    <div class="grid-container">
        <div class="grid" id="grid">
            <div class="header"></div>
            <div class="header">Insuffisant</div>
            <div class="header">Faible</div>
            <div class="header">Satisfaisant</div>
            <div class="header">Très bonne maîtrise</div>
            <div class="header">Forme de jeu</div>
            <div class="cell" data-index="0" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="1" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="2" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="3" onclick="placeCriteria(this)"></div>
            <div class="header">Classement</div>
            <div class="cell" data-index="4" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="5" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="6" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="7" onclick="placeCriteria(this)"></div>
            <div class="header">Porteur de balle</div>
            <div class="cell" data-index="8" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="9" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="10" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="11" onclick="placeCriteria(this)"></div>
            <div class="header">Non porteur de balle</div>
            <div class="cell" data-index="12" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="13" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="14" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="15" onclick="placeCriteria(this)"></div>
            <div class="header">Défense</div>
            <div class="cell" data-index="16" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="17" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="18" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="19" onclick="placeCriteria(this)"></div>
            <div class="header">Attitude</div>
            <div class="cell" data-index="20" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="21" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="22" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="23" onclick="placeCriteria(this)"></div>
            <div class="header">Connaissance</div>
            <div class="cell" data-index="24" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="25" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="26" onclick="placeCriteria(this)"></div>
            <div class="cell" data-index="27" onclick="placeCriteria(this)"></div>
        </div>
    </div>
    <p id="result"></p>
    
    <script>
        const criteriaColumns = [
            ["Grappe ; Orienté et déplacements vers le but adverse", "Regroupés autour du ballon ; Pertes de balles +++", "Jeu écarté ; Ballons et joueurs circulent sur toute la largeur du terrain ; Beaucoup de buts", "Gagne terrain ; Ballon et joueurs circulent d’un but à l’autre ; Alternance attaque – défense ; Peu de buts"],
            ["3ème", "1er", "4ème", "2ème"],
            ["Je ne m’oriente pas vers le but adverse ; Je perds la balle ; Mauvaise passe", "Je marque un but ; Passe précise et stratégique", "Je progresse vers le but adverse ; Je tire dans le but ; Passe précise", "Je m’oriente vers le but adverse ; Je ne progresse pas vers l’avant ; Passe correcte"],
            ["Immobile ; Mauvaise réception", "Je vais vers le ballon ; Réception correcte", "J’enchaine les démarquages et viens en aide au PB ; Réceptionne et enchaine", "Je me démarque à distance de passe vers l’avant ; Réception maitrisée"],
            ["Je défends sur le PB ; Placement maitrisé", "Pas de défense", "J’essaie de défendre ; Placement pas toujours correct", "Je défends efficacement et je récupère le ballon"],
            ["Respecte les consignes, cherche à progresser, joue en équipe", "Investi, motivé, encourage ses partenaires, esprit d’équipe", "Ne respecte pas les consignes, ne coopère pas", "Respecte partiellement les consignes, attitude peu engagée"],
            ["Connaît et applique les règles de base, Observation et arbitrage acquis", "Connaît quelques règles mais ne les applique pas bien", "Applique les règles avec autonomie, est capable d’observer et d’arbitrer une équipe sans commettre d’erreurs", "Ne connait pas les règles"]
        ];
        let selectedCriteria = null;
        let correctAnswers = {};
        let currentColumn = 0;
        let placedCount = 0;

        document.addEventListener("DOMContentLoaded", () => {
            showCurrentRowCriteria();
        });

        function showCurrentRowCriteria() {
            const criteriaContainer = document.getElementById("criteria-container");
            criteriaContainer.innerHTML = "";
            criteriaColumns[currentColumn].forEach(criteria => {
                let div = document.createElement("div");
                div.className = "criteria-box";
                div.innerText = criteria;
                div.onclick = () => selectCriteria(criteria, div);
                criteriaContainer.appendChild(div);
            });
        }

        function selectCriteria(criteria, element) {
            selectedCriteria = criteria;
            document.querySelectorAll(".criteria-box").forEach(box => box.style.background = "lightblue");
            element.style.background = "lightcoral";
        }

        function placeCriteria(cell) {
            if (selectedCriteria && !cell.innerText) {
                cell.innerText = selectedCriteria;
                correctAnswers[cell.dataset.index] = selectedCriteria;
                placedCount++;
                updateCriteriaContainer();
                selectedCriteria = null;
                if (Object.keys(correctAnswers).filter(key => Math.floor(key / 4) === currentColumn).length === 4) {
                    currentColumn++;
                    showCurrentRowCriteria();
                }
            } else if (cell.innerText) {
                let criteria = cell.innerText;
                cell.innerText = "";
                delete correctAnswers[cell.dataset.index];
                placedCount--;
                const criteriaContainer = document.getElementById("criteria-container");
                let div = document.createElement("div");
                div.className = "criteria-box";
                div.innerText = criteria;
                div.onclick = () => selectCriteria(criteria, div);
                criteriaContainer.appendChild(div);
            }
        }

        function updateCriteriaContainer() {
            const criteriaContainer = document.getElementById("criteria-container");
            criteriaContainer.innerHTML = "";
            criteriaColumns[currentColumn].forEach(criteria => {
                if (!Object.values(correctAnswers).includes(criteria)) {
                    let div = document.createElement("div");
                    div.className = "criteria-box";
                    div.innerText = criteria;
                    div.onclick = () => selectCriteria(criteria, div);
                    criteriaContainer.appendChild(div);
                }
            });
        }
    </script>
</body>
</html>
