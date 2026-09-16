[gemini-code-1789545924152 (2).html](https://github.com/user-attachments/files/32278829/gemini-code-1789545924152.2.html)

<!DOCTYPE html>
<html lang="da">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dansk Ord-Hukommelse & Fejlanalyse</title>
    <!-- Chart.js til grafer -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary: #4f46e5;
            --primary-hover: #4338ca;
            --bg: #f8fafc;
            --card-bg: #ffffff;
            --text: #0f172a;
            --text-muted: #64748b;
            --success: #10b981;
            --danger: #ef4444;
            --border: #e2e8f0;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background-color: var(--bg);
            color: var(--text);
            padding: 20px;
            max-width: 900px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            margin-bottom: 30px;
        }

        h1 {
            color: var(--primary);
            margin-bottom: 8px;
        }

        .container {
            background: var(--card-bg);
            padding: 24px;
            border-radius: 16px;
            box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1), 0 2px 4px -2px rgba(0,0,0,0.1);
            margin-bottom: 24px;
        }

        .controls {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 16px;
            margin-bottom: 24px;
        }

        .form-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        label {
            font-weight: 600;
            font-size: 0.9rem;
            color: var(--text-muted);
        }

        input, select, button, textarea {
            padding: 10px 14px;
            border: 1px solid var(--border);
            border-radius: 8px;
            font-size: 1rem;
            outline: none;
        }

        input:focus, select:focus, textarea:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.2);
        }

        button {
            background-color: var(--primary);
            color: white;
            border: none;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        button:hover {
            background-color: var(--primary-hover);
        }

        button.secondary {
            background-color: #e2e8f0;
            color: var(--text);
        }

        button.secondary:hover {
            background-color: #cbd5e1;
        }

        /* Progress Bar for timer */
        .progress-bar-container {
            width: 100%;
            max-width: 350px;
            height: 6px;
            background-color: #e2e8f0;
            border-radius: 3px;
            overflow: hidden;
            margin-top: 10px;
        }

        .progress-bar {
            height: 100%;
            width: 100%;
            background-color: var(--primary);
            transition: width 0.05s linear;
        }

        /* Flashcard Styling */
        .flashcard-area {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 12px;
            margin: 20px 0;
        }

        .card {
            width: 100%;
            max-width: 350px;
            height: 200px;
            perspective: 1000px;
            cursor: pointer;
        }

        .card-inner {
            position: relative;
            width: 100%;
            height: 100%;
            text-align: center;
            transition: transform 0.6s;
            transform-style: preserve-3d;
            box-shadow: 0 10px 15px -3px rgba(0,0,0,0.1);
            border-radius: 12px;
            border: 2px solid var(--border);
        }

        .card.flipped .card-inner {
            transform: rotateY(180deg);
        }

        .card-front, .card-back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            font-weight: bold;
            border-radius: 12px;
            background: white;
            letter-spacing: 2px;
        }

        .card-front {
            color: var(--primary);
        }

        .card-front.hidden-word {
            color: #cbd5e1;
            font-size: 1.5rem;
            font-weight: normal;
            letter-spacing: normal;
        }

        .card-back {
            transform: rotateY(180deg);
            background: #f1f5f9;
            color: var(--text-muted);
            font-size: 1.2rem;
            letter-spacing: normal;
        }

        .card-nav {
            display: flex;
            gap: 12px;
            align-items: center;
            margin-top: 10px;
        }

        /* Test Section & Word Items */
        .test-words-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-top: 12px;
        }

        .word-item {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: space-between;
            padding: 12px;
            background: #f8fafc;
            border-radius: 8px;
            border: 1px solid var(--border);
            gap: 10px;
        }

        .error-select {
            padding: 6px 10px;
            font-size: 0.9rem;
            border-radius: 6px;
            border: 1px solid var(--border);
            background: white;
        }

        /* Tabs for Grafer */
        .tab-buttons {
            display: flex;
            gap: 8px;
            margin-top: 12px;
            margin-bottom: 16px;
            flex-wrap: wrap;
        }

        .tab-btn {
            background-color: #f1f5f9;
            color: var(--text-muted);
            border: 1px solid var(--border);
            padding: 8px 16px;
            border-radius: 6px;
            font-size: 0.9rem;
        }

        .tab-btn.active {
            background-color: var(--primary);
            color: white;
            border-color: var(--primary);
        }

        .chart-container {
            position: relative;
            height: 320px;
            width: 100%;
        }

        .hidden {
            display: none !important;
        }

        .actions {
            display: flex;
            gap: 12px;
            margin-top: 16px;
        }
    </style>
</head>
<body>

    <header>
        <h1>Dansk Hukommelsestræner</h1>
        <p>Baseret på renset ordliste (899 ord).</p>
    </header>

    <!-- Opsætning -->
    <div class="container" id="setup-card">
        <h2>1. Indstillinger</h2>
        <div class="controls" style="margin-top: 16px;">
            <div class="form-group">
                <label for="word-count">Antal ord (ud af 899):</label>
                <input type="number" id="word-count" value="5" min="1" max="50">
            </div>
            <div class="form-group">
                <label for="timer-seconds">Sekunder ml. ord:</label>
                <input type="number" id="timer-seconds" value="3" min="1" max="10">
            </div>
            <div class="form-group">
                <label for="flash-duration">Flash varighed:</label>
                <select id="flash-duration">
                    <option value="300">0.3 sekunder (Meget hurtigt)</option>
                    <option value="500" selected>0.5 sekunder (Hurtigt)</option>
                    <option value="800">0.8 sekunder (Mellem)</option>
                    <option value="1000">1.0 sekund (Langsvarigt)</option>
                </select>
            </div>
        </div>
        <button id="start-btn" style="width: 100%;">Start Flashcards</button>
    </div>

    <!-- Flashcards Visning -->
    <div class="container hidden" id="flashcard-card">
        <h2>2. Husk ordene (Kort <span id="current-card-num">1</span> af <span id="total-cards-num">5</span>)</h2>
        <div class="flashcard-area">
            <div class="card" id="card">
                <div class="card-inner">
                    <div class="card-front" id="card-word">Ord</div>
                    <div class="card-back">Klik for at se ordet igen</div>
                </div>
            </div>
            
            <div class="progress-bar-container">
                <div class="progress-bar" id="progress-bar"></div>
            </div>

            <p style="font-size: 0.85rem; color: var(--text-muted);" id="status-msg">Klik på kortet for at afsløre ordet igen</p>
            
            <div class="card-nav">
                <button id="prev-btn" class="secondary">Forrige</button>
                <button id="toggle-play-btn" class="secondary">⏸ Pause</button>
                <button id="next-btn">Næste</button>
            </div>
        </div>
        <button id="go-to-test-btn" style="width: 100%; margin-top: 16px;">Gå til Test</button>
    </div>

    <!-- Test & Evaluering -->
    <div class="container hidden" id="test-card">
        <h2>3. Test din hukommelse & Kategori-fejl</h2>
        <p style="color: var(--text-muted); margin-bottom: 12px;">Skriv de ord du kan huske i feltet herunder, eller vælg resultat/fejltype direkte på listen:</p>
        
        <div class="form-group">
            <label for="user-input-text">Dine ord (adskilt af komma eller linjeskift):</label>
            <textarea id="user-input-text" rows="3" placeholder="f.eks. abe, bil, hus..."></textarea>
            <button id="auto-check-btn" class="secondary" style="margin-top: 8px;">Tjek mine svar i fritekst</button>
        </div>

        <h3 style="margin-top: 20px;">Vælg resultat eller fejltype for hvert ord:</h3>
        <div class="test-words-list" id="test-words-list">
            <!-- Ord genereres dynamisk her -->
        </div>

        <button id="save-results-btn" style="width: 100%; margin-top: 20px;">Gem Resultat</button>
    </div>

    <!-- Statistik & Grafer -->
    <div class="container">
        <h2>Historik & Fejlanalyse</h2>
        
        <div class="tab-buttons">
            <button class="tab-btn active" id="tab-position-btn">Succes pr. Ord-position (%)</button>
            <button class="tab-btn" id="tab-errors-btn">Fejltype Fordeling</button>
            <button class="tab-btn" id="tab-time-btn">Historik over tid (%)</button>
        </div>

        <div class="chart-container" id="chart-pos-wrapper">
            <canvas id="positionChart"></canvas>
        </div>
        <div class="chart-container hidden" id="chart-errors-wrapper">
            <canvas id="errorTypeChart"></canvas>
        </div>
        <div class="chart-container hidden" id="chart-time-wrapper">
            <canvas id="historyChart"></canvas>
        </div>

        <div class="actions">
            <button id="download-csv-btn" class="secondary">Download CSV Data</button>
            <button id="clear-data-btn" class="secondary" style="color: var(--danger);">Ryd Historik</button>
        </div>
    </div>

    <script>
        // Ordliste genereret direkte fra ord_renset.csv (899 ord)
        const danishWords = [
            "abe", "aer", "aes", "aet", "age", "agn", "agt", "air", "ais", "aks", "akt", "alf", "alk", "alp", "als", "alt", "amt", "and", "ane", "ank", "ara", "ard", "arg", "ark", "arm", "arp", "ars", "art", "arv", "ase", "ask", "asp", "ave", "avl", "boa", "dia", "eja", "eta", "fra", "loa", "rya", "spa", "via", "æra", "bad", "bag", "bak", "bal", "ban", "bar", "bas", "bat", "bed", "beg", "bel", "ben", "beo", "bet", "bid", "bie", "bil", "bio", "bip", "bis", "bit", "ble", "blu", "bly", "blå", "bob", "bod", "bog", "boj", "bol", "bom", "bon", "bop", "bor", "bov", "bro", "bræ", "bud", "bue", "bug", "buk", "bul", "bum", "bur", "bus", "but", "byg", "båd", "bål", "bås", "båt", "bæk", "bæl", "bær", "bæv", "bød", "bøf", "bøg", "bøn", "bør", "bøs", "dub", "dyb", "dåb", "fob", "gab", "gib", "hib", "hob", "håb", "job", "køb", "lab", "lob", "løb", "næb", "peb", "pib", "pub", "reb", "rib", "rub", "råb", "ræb", "søb", "tab", "cam", "ces", "cif", "cis", "cup", "cut", "tic", "dag", "dal", "dam", "dan", "das", "dat", "dav", "dej", "del", "dem", "den", "der", "des", "det", "did", "die", "dig", "dik", "din", "dip", "dir", "dis", "dit", "dog", "dok", "dom", "dop", "dos", "dot", "dræ", "due", "dug", "duk", "dum", "dun", "duo", "dup", "dur", "dus", "dut", "duv", "dyd", "dyk", "dyn", "dyp", "dyr", "dyt", "dåd", "dån", "dæk", "død", "døm", "døn", "dør", "døs", "døv", "end", "fad", "fed", "fod", "fød", "gad", "ged", "gid", "god", "gød", "had", "hed", "hid", "hud", "ild", "ind", "jod", "ked", "kid", "kod", "kåd", "kød", "lad", "led", "lid", "lod", "lud", "lyd", "lød", "mad", "med", "mod", "ned", "nid", "nåd", "nød", "ond", "ord", "rad", "red", "rid", "rod", "råd", "ræd", "rød", "sad", "sid", "sod", "syd", "sæd", "sød", "tid", "tud", "tyd", "uld", "vad", "ved", "vid", "vod", "våd", "ånd", "ege", "egn", "ego", "egs", "egu", "eis", "eje", "eks", "elg", "elm", "els", "elv", "ems", "emu", "ene", "eng", "ens", "epo", "erg", "esp", "evn", "gie", "hue", "ide", "ile", "ise", "kue", "lue", "nye", "ode", "ose", "pie", "roe", "ske", "sne", "spe", "tee", "tie", "tre", "tue", "ude", "uge", "ure", "vie", "vue", "yde", "åle", "åre", "åse", "æde", "ære", "øde", "øge", "øje", "øre", "øse", "øve", "fag", "fan", "far", "fas", "fat", "fax", "fej", "fem", "fer", "fes", "fez", "fif", "fik", "fil", "fim", "fin", "fip", "fis", "fix", "fly", "flå", "fog", "fok", "fol", "fon", "for", "fos", "fri", "fro", "fru", "frø", "fug", "ful", "fup", "fus", "fut", "fyr", "får", "fås", "fæl", "føg", "føj", "føl", "føn", "før", "gaf", "guf", "hof", "kuf", "laf", "luf", "møf", "paf", "pif", "puf", "ruf", "tuf", "tøf", "gag", "gak", "gal", "gas", "gat", "gav", "gel", "gem", "gen", "ges", "gik", "gin", "gir", "gis", "giv", "glo", "gnu", "gny", "gok", "gom", "gro", "gru", "gry", "grå", "gul", "gus", "gut", "gys", "går", "gås", "gæk", "gær", "gæs", "gæt", "gæv", "gøg", "gør", "gøs", "hag", "hug", "hæg", "høg", "jag", "jeg", "jog", "kag", "kig", "kog", "kåg", "lag", "leg", "lig", "log", "lug", "låg", "læg", "løg", "mag", "mig", "mug", "myg", "møg", "nag", "neg", "peg", "pig", "pog", "rag", "rig", "rug", "ryg", "røg", "sag", "sig", "sng", "sug", "syg", "tag", "tog", "tyg", "ung", "vag", "veg", "vig", "væg", "haj", "hak", "hal", "ham", "han", "hap", "har", "has", "hat", "hav", "hel", "hen", "hep", "her", "hev", "hik", "hil", "hin", "hip", "his", "hit", "hiv", "hop", "hor", "hos", "hot", "hov", "huk", "hul", "hun", "hus", "hvo", "hyl", "hyp", "hyr", "hys", "hån", "hår", "hæk", "hæl", "hær", "hæs", "hæv", "høj", "hør", "ido", "ils", "ilt", "ion", "isy", "itu", "kvi", "pli", "ski", "thi", "udi", "jam", "jan", "jas", "jer", "jet", "jok", "jon", "jul", "kaj", "lej", "løj", "maj", "nej", "sej", "tøj", "vej", "kam", "kan", "kap", "kar", "kat", "kel", "ker", "kik", "kil", "kim", "kip", "kir", "kis", "kit", "kiv", "klo", "klø", "kno", "kny", "knæ", "kok", "kom", "kop", "kor", "kro", "kry", "kræ", "kuk", "kul", "kun", "kup", "kur", "kys", "kål", "kår", "kæk", "kæp", "kær", "kæv", "køl", "køn", "kør", "køs", "lak", "lok", "luk", "læk", "mok", "muk", "nak", "nik", "nok", "nyk", "nøk", "ork", "pak", "pik", "rak", "rok", "ryk", "sok", "suk", "sæk", "tak", "tik", "tyk", "ulk", "væk", "yak", "ynk", "lal", "lam", "lap", "las", "lav", "lem", "len", "ler", "les", "let", "lev", "lex", "lim", "lin", "lir", "lis", "liv", "lix", "llo", "lom", "lop", "los", "lov", "lun", "lup", "lur", "lus", "lut", "luv", "lux", "lyn", "lyr", "lys", "lyv", "lån", "lår", "lås", "læn", "lær", "læs", "løn", "løs", "løv", "mel", "mil", "mol", "mål", "mæl", "møl", "nul", "nål", "nøl", "pal", "pil", "pol", "pul", "pæl", "pøl", "ral", "ril", "rul", "sal", "sol", "sul", "syl", "sål", "sæl", "tal", "til", "tyl", "tål", "val", "vel", "vil", "ævl", "mas", "mat", "men", "mim", "min", "mis", "mit", "mon", "mop", "mor", "mos", "mur", "mus", "mut", "myr", "mår", "mås", "mær", "mæt", "mør", "møs", "møv", "nem", "ohm", "olm", "orm", "ram", "rem", "rim", "rom", "rum", "som", "sum", "søm", "tam", "tom", "vom", "nap", "nar", "nas", "nat", "nav", "net", "nev", "nip", "nit", "nop", "nor", "nos", "not", "nus", "nyn", "nys", "nyt", "når", "nås", "næn", "nær", "næs", "nøs", "ovn", "pen", "pin", "pæn", "ran", "ren", "run", "røn", "sen", "sin", "syn", "søn", "ten", "tin", "ton", "tun", "van", "ven", "vin", "von", "væn", "yen", "yin", "zen", "åen", "æon", "øen", "ørn", "obo", "obs", "ofr", "oms", "oss", "ost", "out", "plo", "pro", "rho", "sko", "sno", "tao", "tro", "uro", "uso", "zoo", "øbo", "pap", "par", "pas", "pat", "pep", "per", "pet", "pip", "pir", "pis", "pop", "pos", "pot", "præ", "pur", "pus", "put", "pøs", "rap", "sip", "tap", "tip", "top", "vip", "rar", "rat", "rav", "ret", "rev", "rir", "ris", "ror", "ros", "rov", "rur", "rus", "rut", "rys", "råt", "rær", "ræs", "ræv", "rør", "røv", "sar", "ser", "sir", "sur", "syr", "sår", "sær", "ter", "tor", "tur", "tyr", "tår", "tær", "tør", "uår", "var", "vir", "vor", "vår", "zar", "øer", "sat", "sav", "sax", "ses", "set", "sex", "sgu", "sis", "sit", "siv", "sky", "slå", "små", "snu", "sot", "sov", "spy", "spå", "sus", "sut", "syv", "sås", "såt", "sæt", "søs", "tis", "tus", "tys", "tås", "tøs", "ves", "vis", "vås", "ågs", "tav", "tit", "tot", "tov", "træ", "tut", "tyv", "tæt", "tæv", "tøv", "urt", "vat", "ædt", "ægt", "ært", "ømt", "ørt", "øst", "ulv", "viv", "vov", "vrå", "væv"
        ];

        const errorCategories = {
            correct: "Korrekt 🟢",
            omission: "Omission (Udeladelse/Glemt)",
            intrusion: "Intrusion (Nyt/fremmed ord)",
            substitution: "Substitution (Erstattet ord)",
            transposition: "Transposition (Byttet om)",
            phonological: "Phonological confusion (Lyd-forveksling)",
            visual: "Visual confusion (Syns-forveksling)"
        };

        let activeWords = [];
        let currentIndex = 0;
        let wordStatus = [];
        let historyData = JSON.parse(localStorage.getItem('memory_app_history_v3') || '[]');
        
        let historyChartInstance = null;
        let positionChartInstance = null;
        let errorChartInstance = null;

        // Timere
        let progressInterval = null;
        let flashTimeout = null;
        let isPaused = false;
        let wordDurationSec = 3;
        let flashDurationMs = 500;
        let timeRemainingMs = 3000;

        // DOM Elementer
        const setupCard = document.getElementById('setup-card');
        const flashcardCard = document.getElementById('flashcard-card');
        const testCard = document.getElementById('test-card');
        
        const cardEl = document.getElementById('card');
        const cardWordEl = document.getElementById('card-word');
        const currentCardNum = document.getElementById('current-card-num');
        const totalCardsNum = document.getElementById('total-cards-num');
        const progressBar = document.getElementById('progress-bar');
        const togglePlayBtn = document.getElementById('toggle-play-btn');
        const statusMsg = document.getElementById('status-msg');

        document.addEventListener('DOMContentLoaded', () => {
            initCharts();
        });

        // Tab Navigation
        document.getElementById('tab-position-btn').addEventListener('click', () => switchTab('pos'));
        document.getElementById('tab-errors-btn').addEventListener('click', () => switchTab('errors'));
        document.getElementById('tab-time-btn').addEventListener('click', () => switchTab('time'));

        function switchTab(tab) {
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            document.getElementById('chart-pos-wrapper').classList.add('hidden');
            document.getElementById('chart-errors-wrapper').classList.add('hidden');
            document.getElementById('chart-time-wrapper').classList.add('hidden');

            if (tab === 'pos') {
                document.getElementById('tab-position-btn').classList.add('active');
                document.getElementById('chart-pos-wrapper').classList.remove('hidden');
            } else if (tab === 'errors') {
                document.getElementById('tab-errors-btn').classList.add('active');
                document.getElementById('chart-errors-wrapper').classList.remove('hidden');
            } else if (tab === 'time') {
                document.getElementById('tab-time-btn').classList.add('active');
                document.getElementById('chart-time-wrapper').classList.remove('hidden');
            }
        }

        // Start Flashcards
        document.getElementById('start-btn').addEventListener('click', () => {
            const count = parseInt(document.getElementById('word-count').value) || 5;
            wordDurationSec = parseInt(document.getElementById('timer-seconds').value) || 3;
            flashDurationMs = parseInt(document.getElementById('flash-duration').value) || 500;

            activeWords = getRandomWords(count);
            currentIndex = 0;
            wordStatus = activeWords.map((w, idx) => ({ word: w, position: idx + 1, resultType: 'omission' }));

            setupCard.classList.add('hidden');
            flashcardCard.classList.remove('hidden');
            testCard.classList.add('hidden');

            isPaused = false;
            togglePlayBtn.textContent = "⏸ Pause";

            triggerWordFlash();
            startTimer();
        });

        function getRandomWords(count) {
            let pool = [...danishWords];
            pool.sort(() => Math.random() - 0.5);
            return pool.slice(0, Math.min(count, pool.length));
        }

        cardEl.addEventListener('click', () => {
            cardEl.classList.toggle('flipped');
        });

        function triggerWordFlash() {
            clearTimeout(flashTimeout);
            cardEl.classList.remove('flipped');
            
            cardWordEl.textContent = activeWords[currentIndex];
            cardWordEl.classList.remove('hidden-word');
            
            currentCardNum.textContent = currentIndex + 1;
            totalCardsNum.textContent = activeWords.length;
            statusMsg.textContent = "Klik på kortet for at afsløre ordet igen";

            flashTimeout = setTimeout(() => {
                if (!cardEl.classList.contains('flipped')) {
                    cardWordEl.textContent = "???";
                    cardWordEl.classList.add('hidden-word');
                }
            }, flashDurationMs);
        }

        function startTimer() {
            stopTimer();
            timeRemainingMs = wordDurationSec * 1000;
            const stepMs = 50;

            progressInterval = setInterval(() => {
                if (!isPaused) {
                    timeRemainingMs -= stepMs;
                    let percentage = (timeRemainingMs / (wordDurationSec * 1000)) * 100;
                    progressBar.style.width = `${Math.max(0, percentage)}%`;

                    if (timeRemainingMs <= 0) {
                        nextCardAuto();
                    }
                }
            }, stepMs);
        }

        function stopTimer() {
            if (progressInterval) clearInterval(progressInterval);
            clearTimeout(flashTimeout);
        }

        function nextCardAuto() {
            if (currentIndex < activeWords.length - 1) {
                currentIndex++;
                triggerWordFlash();
                startTimer();
            } else {
                stopTimer();
                progressBar.style.width = '0%';
                statusMsg.textContent = "Alle ord er fremvist. Klik 'Gå til Test' når du er klar!";
            }
        }

        togglePlayBtn.addEventListener('click', () => {
            isPaused = !isPaused;
            togglePlayBtn.textContent = isPaused ? "▶ Afspil" : "⏸ Pause";
        });

        document.getElementById('next-btn').addEventListener('click', () => {
            if (currentIndex < activeWords.length - 1) {
                currentIndex++;
                triggerWordFlash();
                startTimer();
            } else {
                stopTimer();
                goToTest();
            }
        });

        document.getElementById('prev-btn').addEventListener('click', () => {
            if (currentIndex > 0) {
                currentIndex--;
                triggerWordFlash();
                startTimer();
            }
        });

        document.getElementById('go-to-test-btn').addEventListener('click', () => {
            stopTimer();
            goToTest();
        });

        // Test Sektion
        function goToTest() {
            stopTimer();
            flashcardCard.classList.add('hidden');
            testCard.classList.remove('hidden');
            renderTestList();
        }

        function renderTestList() {
            const listEl = document.getElementById('test-words-list');
            listEl.innerHTML = '';

            wordStatus.forEach((item, idx) => {
                const div = document.createElement('div');
                div.className = 'word-item';
                
                let optionsHtml = '';
                for (let key in errorCategories) {
                    const selected = item.resultType === key ? 'selected' : '';
                    optionsHtml += `<option value="${key}" ${selected}>${errorCategories[key]}</option>`;
                }

                div.innerHTML = `
                    <div>
                        <span style="font-weight: 700; color: var(--primary); margin-right: 8px;">Ord ${item.position}:</span>
                        <span style="font-weight: 600; font-size: 1.1rem; letter-spacing: 1px;">${item.word}</span>
                    </div>
                    <div>
                        <select class="error-select" onchange="setResultType(${idx}, this.value)">
                            ${optionsHtml}
                        </select>
                    </div>
                `;
                listEl.appendChild(div);
            });
        }

        window.setResultType = function(index, type) {
            wordStatus[index].resultType = type;
        };

        document.getElementById('auto-check-btn').addEventListener('click', () => {
            const text = document.getElementById('user-input-text').value.toLowerCase();
            const enteredWords = text.split(/[\n,\s]+/).map(w => w.trim()).filter(w => w.length > 0);

            wordStatus.forEach(item => {
                if (enteredWords.includes(item.word.toLowerCase())) {
                    item.resultType = 'correct';
                }
            });
            renderTestList();
        });

        // Gem Resultat
        document.getElementById('save-results-btn').addEventListener('click', () => {
            const rememberedCount = wordStatus.filter(w => w.resultType === 'correct').length;
            const totalCount = wordStatus.length;
            const scorePct = Math.round((rememberedCount / totalCount) * 100);

            const sessionRecord = {
                date: new Date().toLocaleString('da-DK'),
                total: totalCount,
                remembered: rememberedCount,
                scorePct: scorePct,
                details: [...wordStatus]
            };

            historyData.push(sessionRecord);
            localStorage.setItem('memory_app_history_v3', JSON.stringify(historyData));

            updateCharts();
            
            testCard.classList.add('hidden');
            setupCard.classList.remove('hidden');
            document.getElementById('user-input-text').value = '';
            alert(`Resultat gemt! Du huskede ${rememberedCount} ud af ${totalCount} ord (${scorePct}%).`);
        });

        // Statistiske beregninger
        function calculatePositionStats() {
            const positionStats = {};
            historyData.forEach(session => {
                if (session.details) {
                    session.details.forEach(item => {
                        const pos = item.position;
                        if (!positionStats[pos]) {
                            positionStats[pos] = { total: 0, remembered: 0 };
                        }
                        positionStats[pos].total += 1;
                        if (item.resultType === 'correct') {
                            positionStats[pos].remembered += 1;
                        }
                    });
                }
            });

            const maxPos = Math.max(...Object.keys(positionStats).map(Number), 0);
            const labels = [];
            const percentages = [];

            for (let i = 1; i <= maxPos; i++) {
                labels.push(`Ord ${i}`);
                if (positionStats[i] && positionStats[i].total > 0) {
                    const pct = Math.round((positionStats[i].remembered / positionStats[i].total) * 100);
                    percentages.push(pct);
                } else {
                    percentages.push(0);
                }
            }
            return { labels, percentages };
        }

        function calculateErrorStats() {
            const counts = {
                omission: 0,
                intrusion: 0,
                substitution: 0,
                transposition: 0,
                phonological: 0,
                visual: 0
            };

            historyData.forEach(session => {
                if (session.details) {
                    session.details.forEach(item => {
                        if (item.resultType && item.resultType !== 'correct') {
                            counts[item.resultType] = (counts[item.resultType] || 0) + 1;
                        }
                    });
                }
            });

            const labels = [
                'Omission', 'Intrusion', 'Substitution', 
                'Transposition', 'Phonological', 'Visual'
            ];
            const data = [
                counts.omission, counts.intrusion, counts.substitution, 
                counts.transposition, counts.phonological, counts.visual
            ];

            return { labels, data };
        }

        // Initialize Charts
        function initCharts() {
            // 1. Position Chart
            const posCtx = document.getElementById('positionChart').getContext('2d');
            const posData = calculatePositionStats();
            positionChartInstance = new Chart(posCtx, {
                type: 'bar',
                data: {
                    labels: posData.labels,
                    datasets: [{
                        label: 'Succesrate pr. Position (%)',
                        data: posData.percentages,
                        backgroundColor: 'rgba(79, 70, 229, 0.7)',
                        borderColor: '#4f46e5',
                        borderWidth: 1,
                        borderRadius: 6
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: { y: { min: 0, max: 100, title: { display: true, text: 'Husket i procent (%)' } } }
                }
            });

            // 2. Error Type Chart
            const errCtx = document.getElementById('errorTypeChart').getContext('2d');
            const errData = calculateErrorStats();
            errorChartInstance = new Chart(errCtx, {
                type: 'doughnut',
                data: {
                    labels: errData.labels,
                    datasets: [{
                        label: 'Antal fejl',
                        data: errData.data,
                        backgroundColor: [
                            '#ef4444', '#f97316', '#f59e0b', 
                            '#8b5cf6', '#ec4899', '#06b6d4'
                        ]
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { position: 'bottom' },
                        title: { display: true, text: 'Fordeling af Fejltyper' }
                    }
                }
            });

            // 3. Time Chart
            const timeCtx = document.getElementById('historyChart').getContext('2d');
            historyChartInstance = new Chart(timeCtx, {
                type: 'line',
                data: {
                    labels: historyData.map((_, i) => `Test ${i + 1}`),
                    datasets: [{
                        label: 'Samlet Succesrate (%)',
                        data: historyData.map(d => d.scorePct),
                        borderColor: '#10b981',
                        backgroundColor: 'rgba(16, 185, 129, 0.1)',
                        fill: true,
                        tension: 0.2
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: { y: { min: 0, max: 100, title: { display: true, text: 'Procent Husket (%)' } } }
                }
            });
        }

        function updateCharts() {
            if (positionChartInstance && historyChartInstance && errorChartInstance) {
                const posData = calculatePositionStats();
                positionChartInstance.data.labels = posData.labels;
                positionChartInstance.data.datasets[0].data = posData.percentages;
                positionChartInstance.update();

                const errData = calculateErrorStats();
                errorChartInstance.data.labels = errData.labels;
                errorChartInstance.data.datasets[0].data = errData.data;
                errorChartInstance.update();

                historyChartInstance.data.labels = historyData.map((_, i) => `Test ${i + 1}`);
                historyChartInstance.data.datasets[0].data = historyData.map(d => d.scorePct);
                historyChartInstance.update();
            }
        }

        // CSV Eksport
        document.getElementById('download-csv-btn').addEventListener('click', () => {
            if (historyData.length === 0) {
                alert("Der er ingen data at downloade endnu.");
                return;
            }

            let csvContent = "data:text/csv;charset=utf-8,Dato,Test Nr,Ord Position,Ord,Kategori/Fejltype\n";
            historyData.forEach((row, sessionIdx) => {
                if (row.details) {
                    row.details.forEach(item => {
                        const statusText = errorCategories[item.resultType] || item.resultType;
                        csvContent += `"${row.date}",${sessionIdx + 1},${item.position},"${item.word}","${statusText}"\n`;
                    });
                }
            });

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", "hukommelse_fejltype_analyse.csv");
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        });

        document.getElementById('clear-data-btn').addEventListener('click', () => {
            if (confirm("Er du sikker på, at du vil slette al gemt historik?")) {
                historyData = [];
                localStorage.removeItem('memory_app_history_v3');
                updateCharts();
            }
        });
    </script>
</body>
</html>
