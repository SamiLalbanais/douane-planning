<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Répartition de la Douane - Douane Mystique</title>
    <style>
        :root {
            --bg-dark-brown: #0b0711; /* Fond nuit très sombre tirant sur le violet */
            --bg-parchment: #1a1026;   /* Violet/brun sombre pour les grimoires */
            --cell-bg: #251836;        /* Cases du tableau */
            --border-gold: #c5a059;     /* Or vieilli */
            --text-gold: #e2c185;       /* Or clair */
            --text-light: #f5eedc;      /* Crème */
            --color-gryf: #8a1414;      /* Rouge écarlate */
            --color-slyth: #1a472a;     /* Vert émeraude */
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Georgia', serif;
        }

        body {
            background-color: var(--bg-dark-brown);
            /* Véritable fond étoilé fixe, visible et propre */
            background-image: 
                radial-gradient(white, rgba(255,255,255,.2) 2px, transparent 40px),
                radial-gradient(white, rgba(255,255,255,.15) 1px, transparent 30px),
                radial-gradient(white, rgba(255,255,255,.1) 2px, transparent 40px);
            background-size: 550px 550px, 350px 350px, 250px 250px;
            background-position: 0 0, 40px 60px, 130px 270px;
            color: var(--text-light);
            display: flex;
            height: 100vh;
            overflow: hidden;
        }

        /* --- SIDEBAR --- */
        #sidebar {
            width: 320px;
            background-color: var(--bg-parchment);
            border-right: 4px double var(--border-gold);
            padding: 25px 20px;
            display: flex;
            flex-direction: column;
            box-shadow: 5px 0 20px rgba(0,0,0,0.8);
            z-index: 10;
        }

        #sidebar h2 {
            color: var(--text-gold);
            text-align: center;
            font-variant: small-caps;
            margin-bottom: 25px;
            border-bottom: 2px double var(--border-gold);
            padding-bottom: 12px;
            font-size: 1.3rem;
            letter-spacing: 1px;
        }

        .input-group {
            display: flex;
            gap: 8px;
            margin-bottom: 25px;
        }

        #member-input {
            flex: 1;
            padding: 10px;
            border: 1px solid var(--border-gold);
            background: #0b0711;
            color: var(--text-light);
            border-radius: 4px;
            font-size: 0.95rem;
        }

        #member-input::placeholder {
            color: #8a7355;
        }

        #add-btn {
            background-color: var(--color-gryf);
            color: white;
            border: 1px solid var(--border-gold);
            padding: 10px 15px;
            cursor: pointer;
            border-radius: 4px;
            font-weight: bold;
            transition: 0.2s;
        }

        #add-btn:hover {
            background-color: #a61c1c;
            transform: scale(1.02);
        }

        #tags-container {
            flex: 1;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 12px;
            padding: 10px;
            background: rgba(0, 0, 0, 0.4);
            border-radius: 6px;
            border: 1px dashed rgba(197, 160, 89, 0.2);
        }

        /* --- ÉTIQUETTES (TAGS) --- */
        .tag {
            background: linear-gradient(135deg, #d7c297, #b09663);
            border: 1px solid #0b0711;
            border-left: 6px solid var(--color-gryf);
            padding: 12px;
            border-radius: 5px;
            cursor: grab;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-weight: bold;
            box-shadow: 3px 3px 8px rgba(0,0,0,0.5);
            user-select: none;
            color: #160c07;
            transition: transform 0.1s;
        }

        .tag:active {
            cursor: grabbing;
            transform: scale(0.98);
        }

        .tag .delete-btn {
            background: transparent;
            border: none;
            color: #5c4a33;
            cursor: pointer;
            font-weight: bold;
            font-size: 1.3rem;
            line-height: 1;
            margin-left: 10px;
        }

        .tag .delete-btn:hover {
            color: var(--color-gryf);
        }

        /* --- ZONE TABLEAU --- */
        #main-content {
            flex: 1;
            padding: 30px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 25px;
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 2px solid var(--border-gold);
            padding-bottom: 15px;
        }

        .title-container {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        header h1 {
            font-variant: small-caps;
            color: var(--text-gold);
            font-size: 2.2rem;
            letter-spacing: 1px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.8);
        }

        /* Texte fixe 'du' et 'au' avec exactement le même style que le titre */
        .date-label {
            font-variant: small-caps;
            font-size: 2.2rem;
            letter-spacing: 1px;
            font-weight: bold;
            color: var(--text-gold);
            text-shadow: 2px 2px 4px rgba(0,0,0,0.8);
            user-select: none;
        }

        /* Champs de saisie des dates harmonisés */
        .date-input {
            background: transparent;
            border: none;
            color: var(--text-gold);
            font-family: 'Georgia', serif;
            font-variant: small-caps;
            font-size: 2.2rem;
            letter-spacing: 1px;
            font-weight: bold;
            padding: 2px 0px;
            width: 150px;
            text-align: center;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.8);
        }

        .date-input:focus {
            outline: none;
            color: var(--text-light);
        }

        #clear-btn {
            background-color: var(--color-slyth);
            color: var(--text-light);
            border: 1px solid var(--border-gold);
            padding: 10px 20px;
            cursor: pointer;
            border-radius: 4px;
            font-weight: bold;
            transition: background 0.2s;
        }

        #clear-btn:hover {
            background-color: #24633b;
        }

        /* --- TABLEAU --- */
        table {
            width: 100%;
            border-collapse: separate;
            border-spacing: 8px;
            background: rgba(26, 16, 38, 0.85); /* Semi-transparent pour laisser deviner les étoiles derrière */
            border: 3px double var(--border-gold);
            border-radius: 10px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.6);
            backdrop-filter: blur(2px);
        }

        th, td {
            padding: 12px;
            text-align: center;
            vertical-align: top;
            border-radius: 6px;
        }

        th {
            background-color: var(--bg-dark-brown);
            color: var(--text-gold);
            font-variant: small-caps;
            letter-spacing: 1px;
            font-size: 1.1rem;
            border: 1px solid rgba(197, 160, 89, 0.4);
        }

        .row-header {
            background: linear-gradient(to right, #1a1026, #251836);
            color: var(--text-gold);
            font-weight: bold;
            font-size: 1.2rem;
            font-variant: small-caps;
            vertical-align: middle;
            width: 120px;
            border: 1px solid var(--border-gold);
            text-shadow: 1px 1px 2px rgba(0,0,0,0.5);
        }

        /* Séparation visuelle pour la ligne Absent */
        tr.row-absent td {
            border-top: 3px double var(--border-gold);
            padding-top: 20px;
        }

        /* Drop Zones */
        .drop-zone {
            min-height: 140px;
            background-color: var(--cell-bg);
            border: 1px solid rgba(197, 160, 89, 0.15);
            border-radius: 6px;
            transition: background-color 0.2s, border-color 0.2s, box-shadow 0.2s;
            display: flex;
            flex-direction: column;
            gap: 8px;
            padding: 8px;
            box-shadow: inset 0 2px 5px rgba(0,0,0,0.5);
        }

        .drop-zone.drag-over {
            background-color: #34224c;
            border: 2px dashed var(--border-gold);
            box-shadow: inset 0 2px 10px rgba(0,0,0,0.7);
        }

        .drop-zone .tag {
            font-size: 0.9rem;
            padding: 8px 10px;
        }
    </style>
</head>
<body>

    <!-- SIDEBAR -->
    <div id="sidebar">
        <h2>Grimoire des Douaniers</h2>
        <div class="input-group">
            <input type="text" id="member-input" placeholder="Inscrire un membre..." autocomplete="off">
            <button id="add-btn">Invoquer</button>
        </div>
        <div id="tags-container" class="drop-zone" data-zone="pool"></div>
    </div>

    <!-- MAIN CONTENT -->
    <div id="main-content">
        <header>
            <div class="title-container">
                <h1>Planification de la Douane</h1>
                <span class="date-label">du</span>
                <input type="text" id="date-start" class="date-input" placeholder="..." autocomplete="off">
                <span class="date-label">au</span>
                <input type="text" id="date-end" class="date-input" placeholder="..." autocomplete="off">
            </div>
            <button id="clear-btn" onclick="resetTableau()">Vider le Grimoire</button>
        </header>

        <table>
            <thead>
                <tr>
                    <th>Canal</th>
                    <th>Lundi</th>
                    <th>Mardi</th>
                    <th>Mercredi</th>
                    <th>Jeudi</th>
                    <th>Vendredi</th>
                    <th>Samedi</th>
                    <th>Dimanche</th>
                </tr>
            </thead>
            <tbody>
                <!-- LIGNE ÉCRIT -->
                <tr>
                    <td class="row-header">Écrit</td>
                    <td><div class="drop-zone" data-zone="Lundi-Ecrit"></div></td>
                    <td><div class="drop-zone" data-zone="Mardi-Ecrit"></div></td>
                    <td><div class="drop-zone" data-zone="Mercredi-Ecrit"></div></td>
                    <td><div class="drop-zone" data-zone="Jeudi-Ecrit"></div></td>
                    <td><div class="drop-zone" data-zone="Vendredi-Ecrit"></div></td>
                    <td><div class="drop-zone" data-zone="Samedi-Ecrit"></div></td>
                    <td><div class="drop-zone" data-zone="Dimanche-Ecrit"></div></td>
                </tr>
                <!-- LIGNE ORAL -->
                <tr>
                    <td class="row-header">Oral</td>
                    <td><div class="drop-zone" data-zone="Lundi-Oral"></div></td>
                    <td><div class="drop-zone" data-zone="Mardi-Oral"></div></td>
                    <td><div class="drop-zone" data-zone="Mercredi-Oral"></div></td>
                    <td><div class="drop-zone" data-zone="Jeudi-Oral"></div></td>
                    <td><div class="drop-zone" data-zone="Vendredi-Oral"></div></td>
                    <td><div class="drop-zone" data-zone="Samedi-Oral"></div></td>
                    <td><div class="drop-zone" data-zone="Dimanche-Oral"></div></td>
                </tr>
                <!-- LIGNE ABSENT -->
                <tr class="row-absent">
                    <td class="row-header">Absent</td>
                    <td><div class="drop-zone" data-zone="Lundi-Absent"></div></td>
                    <td><div class="drop-zone" data-zone="Mardi-Absent"></div></td>
                    <td><div class="drop-zone" data-zone="Mercredi-Absent"></div></td>
                    <td><div class="drop-zone" data-zone="Jeudi-Absent"></div></td>
                    <td><div class="drop-zone" data-zone="Vendredi-Absent"></div></td>
                    <td><div class="drop-zone" data-zone="Samedi-Absent"></div></td>
                    <td><div class="drop-zone" data-zone="Dimanche-Absent"></div></td>
                </tr>
            </tbody>
        </table>
    </div>

    <script>
        let appData = { 
            pool: [],      
            schedule: [],  
            dateStart: "", 
            dateEnd: ""    
        };

        const input = document.getElementById('member-input');
        const addBtn = document.getElementById('add-btn');
        const dateStartInput = document.getElementById('date-start');
        const dateEndInput = document.getElementById('date-end');

        window.addEventListener('DOMContentLoaded', () => {
            const savedData = localStorage.getItem('douane_schedule_v5');
            if (savedData) {
                appData = JSON.parse(savedData);
                if (!appData.pool) appData = { pool: appData.members || [], schedule: [], dateStart: "", dateEnd: "" };
                
                dateStartInput.value = appData.dateStart || "";
                dateEndInput.value = appData.dateEnd || "";
                
                renderData();
            }
            setupDragAndDrop();

            dateStartInput.addEventListener('input', () => {
                appData.dateStart = dateStartInput.value;
                saveData();
            });
            dateEndInput.addEventListener('input', () => {
                appData.dateEnd = dateEndInput.value;
                saveData();
            });
        });

        function createMember() {
            const name = input.value.trim();
            if (!name) return;

            const newMember = {
                id: 'p_' + Date.now() + '_' + Math.random().toString(36).substr(2, 5),
                name: name
            };

            appData.pool.push(newMember);
            saveData();
            renderData();
            input.value = '';
        }

        addBtn.addEventListener('click', createMember);
        input.addEventListener('keypress', (e) => { if (e.key === 'Enter') createMember(); });

        function deleteFromPool(id, name) {
            appData.pool = appData.pool.filter(m => m.id !== id);
            appData.schedule = appData.schedule.filter(s => s.name !== name);
            saveData();
            renderData();
        }

        function deleteFromSchedule(id) {
            appData.schedule = appData.schedule.filter(s => s.id !== id);
            saveData();
            renderData();
        }

        function saveData() {
            localStorage.setItem('douane_schedule_v5', JSON.stringify(appData));
        }

        function renderData() {
            document.querySelectorAll('.drop-zone').forEach(zone => zone.innerHTML = '');

            const poolZone = document.querySelector('[data-zone="pool"]');
            appData.pool.forEach(member => {
                const tag = document.createElement('div');
                tag.className = 'tag';
                tag.draggable = true;
                tag.id = member.id;
                tag.dataset.source = 'pool';
                tag.dataset.name = member.name;
                tag.innerHTML = `
                    <span>${member.name}</span>
                    <button class="delete-btn" onclick="deleteFromPool('${member.id}', '${member.name}'); event.stopPropagation();">×</button>
                `;
                
                tag.addEventListener('dragstart', (e) => {
                    e.dataTransfer.setData('text/plain', tag.id);
                    e.dataTransfer.setData('source', 'pool');
                });

                poolZone.appendChild(tag);
            });

            appData.schedule.forEach(item => {
                const targetZone = document.querySelector(`[data-zone="${item.zone}"]`);
                if (targetZone) {
                    const tag = document.createElement('div');
                    tag.className = 'tag';
                    tag.draggable = true;
                    tag.id = item.id;
                    tag.dataset.source = 'schedule';
                    tag.innerHTML = `
                        <span>${item.name}</span>
                        <button class="delete-btn" onclick="deleteFromSchedule('${item.id}'); event.stopPropagation();">×</button>
                    `;
                    
                    tag.addEventListener('dragstart', (e) => {
                        e.dataTransfer.setData('text/plain', tag.id);
                        e.dataTransfer.setData('source', 'schedule');
                    });

                    targetZone.appendChild(tag);
                }
            });
        }

        function setupDragAndDrop() {
            const zones = document.querySelectorAll('.drop-zone');

            zones.forEach(zone => {
                zone.addEventListener('dragover', (e) => {
                    e.preventDefault();
                    zone.classList.add('drag-over');
                });

                zone.addEventListener('dragleave', () => {
                    zone.classList.remove('drag-over');
                });

                zone.addEventListener('drop', (e) => {
                    e.preventDefault();
                    zone.classList.remove('drag-over');
                    
                    const id = e.dataTransfer.getData('text/plain');
                    const source = e.dataTransfer.getData('source');
                    const targetZoneAttr = zone.getAttribute('data-zone');

                    if (targetZoneAttr === 'pool') return;

                    if (source === 'pool') {
                        const original = appData.pool.find(m => m.id === id);
                        if (original) {
                            appData.schedule.push({
                                id: 's_' + Date.now() + '_' + Math.random().toString(36).substr(2, 5),
                                name: original.name,
                                zone: targetZoneAttr
                            });
                        }
                    } else if (source === 'schedule') {
                        const item = appData.schedule.find(s => s.id === id);
                        if (item) {
                            item.zone = targetZoneAttr;
                        }
                    }

                    saveData();
                    renderData();
                });
            });
        }

        function resetTableau() {
            appData.schedule = []; // Supprime la répartition
            appData.dateStart = ""; // Efface les dates
            appData.dateEnd = "";
            dateStartInput.value = "";
            dateEndInput.value = "";
            saveData();
            renderData();
        }
    </script>
</body>
</html>
