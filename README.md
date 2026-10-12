# Mensa_Bestellungen

<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CampusBiss - Mensa Vorbestellung</title>
  <style>
    :root {
      --primary-color: #3498db;
      --secondary-color: #27ae60;
      --accent-color: #e67e22;
      --bg-color: #f0f4f8;
      --text-color: #2c3e50;
      --card-bg: #ffffff;
      --border-radius: 12px;
      --shadow: 0 8px 30px rgba(0,0,0,0.08);
    }

    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: var(--bg-color);
      color: var(--text-color);
      margin: 0;
      padding: 20px;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }

    .container {
      max-width: 650px;
      width: 100%;
      background: var(--card-bg);
      padding: 40px;
      border-radius: var(--border-radius);
      box-shadow: var(--shadow);
    }

    .header-section {
      text-align: center;
      margin-bottom: 35px;
      border-bottom: 2px solid #eee;
      padding-bottom: 20px;
    }

    h2 {
      color: var(--primary-color);
      margin: 0 0 10px 0;
      font-size: 2.2em;
      font-weight: 700;
    }

    .subtitle {
      color: #7f8c8d;
      font-size: 1.1em;
      margin: 0;
    }

    .form-group {
      margin-bottom: 25px;
    }

    label.main-label {
      font-weight: 600;
      display: block;
      margin-bottom: 10px;
      color: var(--text-color);
      font-size: 1.05em;
    }

    input[type="text"], 
    input[type="time"], 
    select {
      width: 100%;
      padding: 15px;
      border: 2px solid #e1e8ed;
      border-radius: 8px;
      box-sizing: border-box;
      font-size: 1em;
      transition: border-color 0.3s, box-shadow 0.3s;
      background-color: #fbfcfd;
    }

    input[type="text"]:focus, 
    input[type="time"]:focus, 
    select:focus {
      border-color: var(--primary-color);
      box-shadow: 0 0 0 4px rgba(52, 152, 219, 0.15);
      outline: none;
      background-color: #fff;
    }

    .option-container {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .option-label {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 15px;
      border: 2px solid #e1e8ed;
      border-radius: 8px;
      cursor: pointer;
      transition: background-color 0.2s, border-color 0.2s;
      background-color: #fbfcfd;
    }

    .option-label:hover {
      background-color: #f1f7fc;
      border-color: #d0dbe5;
    }

    .option-label.selected {
      border-color: var(--primary-color);
      background-color: #eafaf1;
    }

    .option-left {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .option-input {
      width: 20px;
      height: 20px;
      accent-color: var(--primary-color);
      cursor: pointer;
    }

    .price-tag {
      font-weight: bold;
      color: #e67e22;
      background: #fef5ec;
      padding: 4px 8px;
      border-radius: 6px;
      font-size: 0.9em;
    }

    .dynamic-section {
      background-color: #fdfae3;
      padding: 20px;
      border-left: 5px solid var(--accent-color);
      border-radius: 8px;
      margin-top: -10px;
      margin-bottom: 25px;
      display: none;
      box-shadow: inset 0 2px 5px rgba(0,0,0,0.05);
    }

    .dynamic-section label {
      color: #856404;
      font-weight: 600;
    }

    .total-box {
      background-color: #e8f8f5;
      border: 2px dashed #27ae60;
      border-radius: 8px;
      padding: 15px 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 25px;
      font-size: 1.2em;
    }

    .total-title {
      font-weight: 600;
      color: #27ae60;
    }

    .total-amount {
      font-weight: 700;
      color: #27ae60;
      font-size: 1.3em;
    }

    .btn-submit {
      background-color: var(--secondary-color);
      color: white;
      border: none;
      padding: 18px 25px;
      font-size: 1.2em;
      border-radius: 8px;
      cursor: pointer;
      width: 100%;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 1px;
      transition: background-color 0.3s, transform 0.1s, box-shadow 0.3s;
      box-shadow: 0 5px 15px rgba(39, 174, 96, 0.2);
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 10px;
    }

    .btn-submit:hover {
      background-color: #219150;
      box-shadow: 0 8px 20px rgba(39, 174, 96, 0.3);
    }

    .btn-submit:disabled {
      background-color: #95a5a6;
      cursor: not-allowed;
      box-shadow: none;
    }

    /* Lade-Spinner Animation */
    .spinner {
      width: 20px;
      height: 20px;
      border: 3px solid rgba(255,255,255,0.3);
      border-radius: 50%;
      border-top-color: white;
      animation: spin 0.8s linear infinite;
      display: none;
    }

    @keyframes spin {
      to { transform: rotate(360deg); }
    }

    /* MODAL / POP-UP BESTÄTIGUNGSFENSTER */
    .modal-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 1000;
      opacity: 0;
      visibility: hidden;
      transition: opacity 0.3s, visibility 0.3s;
    }

    .modal-overlay.active {
      opacity: 1;
      visibility: visible;
    }

    .modal-box {
      background: white;
      padding: 30px;
      border-radius: 12px;
      max-width: 400px;
      width: 90%;
      text-align: center;
      box-shadow: 0 10px 25px rgba(0,0,0,0.2);
      transform: translateY(-20px);
      transition: transform 0.3s;
    }

    .modal-overlay.active .modal-box {
      transform: translateY(0);
    }

    .modal-icon {
      font-size: 3.5em;
      color: #27ae60;
      margin-bottom: 15px;
    }

    .modal-title {
      font-size: 1.5em;
      font-weight: 700;
      color: #2c3e50;
      margin-bottom: 10px;
    }

    .modal-text {
      color: #7f8c8d;
      font-size: 1.05em;
      margin-bottom: 25px;
      line-height: 1.4;
    }

    .modal-btn {
      background-color: var(--secondary-color);
      color: white;
      border: none;
      padding: 12px 20px;
      font-size: 1em;
      border-radius: 6px;
      cursor: pointer;
      font-weight: bold;
      width: 100%;
    }

    .modal-btn:hover {
      background-color: #219150;
    }

    @media (max-width: 600px) {
      .container { padding: 20px; }
      h2 { font-size: 1.8em; }
    }
  </style>
</head>
<body>

<div class="container">
  <div class="header-section">
    <h2>CampusBiss 🍽️</h2>
    <p class="subtitle">Deine Mensa-Vorbestellung mit Live-Bestätigung</p>
  </div>

  <form id="orderForm">
    <!-- Kundendaten -->
    <div class="form-group">
      <label for="name" class="main-label">Name, Vorname *</label>
      <input type="text" id="name" required placeholder="z.B. Max Mustermann">
    </div>

    <div class="form-group">
      <label for="matrikelnummer" class="main-label">Matrikelnummer (6-stellig) *</label>
      <input type="text" id="matrikelnummer" pattern="\d{6}" required placeholder="123456" title="Bitte gib deine 6-stellige Matrikelnummer ein.">
    </div>

    <!-- Hauptgericht mit Preisen -->
    <div class="form-group">
      <label class="main-label">Hauptgericht wählen *</label>
      <div class="option-container">
        
        <label class="option-label" for="gericht1">
          <div class="option-left">
            <input type="radio" name="hauptgericht" id="gericht1" value="Vegetarisch: Gemüselasagne" data-price="4.50" class="option-input" required onclick="handleMealChange(this)">
            <span>🌿 Vegetarisch: Gemüselasagne</span>
          </div>
          <span class="price-tag">4,50 €</span>
        </label>

        <label class="option-label" for="gericht2">
          <div class="option-left">
            <input type="radio" name="hauptgericht" id="gericht2" value="Fleisch: Hähnchencurry mit Reis" data-price="5.20" class="option-input" onclick="handleMealChange(this)">
            <span>🍗 Fleisch: Hähnchencurry mit Reis</span>
          </div>
          <span class="price-tag">5,20 €</span>
        </label>

        <label class="option-label" for="gericht3">
          <div class="option-left">
            <input type="radio" name="hauptgericht" id="gericht3" value="Spezial: Salatteller / Custom Bowl" data-price="4.00" class="option-input" onclick="handleMealChange(this)">
            <span>🥗 Spezial: Individueller Salatteller</span>
          </div>
          <span class="price-tag">4,00 €</span>
        </label>
        
      </div>
    </div>

    <!-- DYNAMISCHER BEREICH -->
    <div id="bowlOptions" class="dynamic-section">
      <div class="form-group" style="margin-bottom: 0;">
        <label for="beilage" class="main-label">Wähle dein Extra-Topping:</label>
        <select id="beilage" onchange="calculateTotal()">
          <option value="" disabled selected data-price="0">Bitte wählen...</option>
          <option value="Falafel" data-price="1.50">Kichererbsen-Falafel (+ 1,50 €)</option>
          <option value="Hähnchenbruststreifen" data-price="2.00">Gegrillte Hähnchenbrust (+ 2,00 €)</option>
          <option value="Tofu" data-price="1.80">Räuchertofu (+ 1,80 €)</option>
        </select>
      </div>
    </div>

    <!-- Desserts mit Preisen -->
    <div class="form-group">
      <label class="main-label">Dessert / Extras (optional)</label>
      <div class="option-container">
        
        <label class="option-label" for="dessert1">
          <div class="option-left">
            <input type="checkbox" name="dessert" id="dessert1" value="Obstsalat" data-price="1.20" class="option-input" onclick="handleCheckboxChange()">
            <span>🍎 Frischer Obstsalat</span>
          </div>
          <span class="price-tag">1,20 €</span>
        </label>

        <label class="option-label" for="dessert2">
          <div class="option-left">
            <input type="checkbox" name="dessert" id="dessert2" value="Schokopudding" data-price="1.50" class="option-input" onclick="handleCheckboxChange()">
            <span>🍫 Schokopudding</span>
          </div>
          <span class="price-tag">1,50 €</span>
        </label>
        
      </div>
    </div>

    <!-- Sonstiges -->
    <div class="form-group">
      <label for="unvertraeglichkeiten" class="main-label">Unverträglichkeiten / Wünsche</label>
      <input type="text" id="unvertraeglichkeiten" placeholder="z. B. laktosefrei, keine Nüsse">
    </div>

    <!-- Abholzeit -->
    <div class="form-group">
      <label for="abholzeit" class="main-label">Gewünschte Abholzeit (11:30 - 14:00 Uhr) *</label>
      <input type="time" id="abholzeit" min="11:30" max="14:00" required>
    </div>

    <!-- GESAMTBETRAG -->
    <div class="total-box">
      <span class="total-title">Gesamtbetrag:</span>
      <span id="totalPrice" class="total-amount">0,00 €</span>
    </div>

    <button type="submit" id="submitBtn" class="btn-submit">
      <span id="btnText">Kostenpflichtig vorbestellen</span>
      <div id="loadingSpinner" class="spinner"></div>
    </button>
  </form>
</div>

<!-- ERFOLGS-MODAL (Pop-up Bestätigung) -->
<div id="successModal" class="modal-overlay">
  <div class="modal-box">
    <div class="modal-icon">✅</div>
    <div class="modal-title">Erfolgreich bestellt!</div>
    <div class="modal-text" id="modalMessage">Deine Vorbestellung wurde erfolgreich an die Mensa übermittelt und in Google Sheets gespeichert. Guten Appetit!</div>
    <button class="modal-btn" onclick="closeModal()">Alles klar</button>
  </div>
</div>

<script>
  // Gesamtpreis berechnen
  function calculateTotal() {
    let total = 0;
    const selectedMeal = document.querySelector('input[name="hauptgericht"]:checked');
    if (selectedMeal) {
      total += parseFloat(selectedMeal.getAttribute('data-price') || 0);
    }

    const bowlOptions = document.getElementById('bowlOptions');
    if (bowlOptions.style.display === 'block') {
      const beilageSelect = document.getElementById('beilage');
      const selectedOption = beilageSelect.options[beilageSelect.selectedIndex];
      if (selectedOption && selectedOption.getAttribute('data-price')) {
        total += parseFloat(selectedOption.getAttribute('data-price'));
      }
    }

    const selectedDesserts = document.querySelectorAll('input[name="dessert"]:checked');
    selectedDesserts.forEach(cb => {
      total += parseFloat(cb.getAttribute('data-price') || 0);
    });

    document.getElementById('totalPrice').innerText = total.toFixed(2).replace('.', ',') + ' €';
    return total.toFixed(2) + ' €';
  }

  function updateOptionLabelStyling(name) {
    const inputs = document.querySelectorAll(`input[name="${name}"]`);
    inputs.forEach(input => {
      const label = input.closest('.option-label');
      if (input.checked) {
        label.classList.add('selected');
      } else {
        label.classList.remove('selected');
      }
    });
  }

  function handleMealChange(radioInput) {
    updateOptionLabelStyling('hauptgericht');
    const selectedMeal = radioInput.value;
    const bowlOptions = document.getElementById('bowlOptions');
    const beilageSelect = document.getElementById('beilage');
    
    if (selectedMeal.includes('Salatteller')) {
      bowlOptions.style.display = 'block';
      beilageSelect.required = true;
    } else {
      bowlOptions.style.display = 'none';
      beilageSelect.required = false;
      beilageSelect.value = '';
    }
    calculateTotal();
  }

  function handleCheckboxChange() {
    updateOptionLabelStyling('dessert');
    calculateTotal();
  }

  // Modal Pop-up steuern
  function showModal(name, time) {
    document.getElementById('modalMessage').innerHTML = 
      `<b>Vielen Dank, ${name}!</b><br>Deine Bestellung für ca. <b>${time} Uhr</b> wurde erfolgreich abgeschickt und in der Mensa registriert.`;
    document.getElementById('successModal').classList.add('active');
  }

  function closeModal() {
    document.getElementById('successModal').classList.remove('active');
  }

  // --- FORMULAR-VERARBEITUNG & ABSENDEN ---
  const scriptURL = 'https://script.google.com/macros/s/AKfycbweN1asQkduCo35lFI_47r--d4zZEPBeDxosX0PIv5z3WjsK9yRd8-il6o5jASbtrAI-A/exec'; // <-- HIER URL EINTRAGEN

  document.getElementById('orderForm').addEventListener('submit', function(e) {
    e.preventDefault();
    
    const submitBtn = document.getElementById('submitBtn');
    const btnText = document.getElementById('btnText');
    const loadingSpinner = document.getElementById('loadingSpinner');
    
    // UI in Ladezustand versetzen (Spinner an)
    submitBtn.disabled = true;
    btnText.innerText = "Wird übertragen...";
    loadingSpinner.style.display = 'inline-block';

    const userName = document.getElementById('name').value;
    const pickupTime = document.getElementById('abholzeit').value;

    const desserts = Array.from(document.querySelectorAll('input[name="dessert"]:checked'))
                          .map(cb => cb.value).join(', ');

    const formData = {
      name: userName,
      matrikelnummer: document.getElementById('matrikelnummer').value,
      hauptgericht: document.querySelector('input[name="hauptgericht"]:checked').value,
      beilage: document.getElementById('beilage').value || "-",
      dessert: desserts || "Keines",
      unvertraeglichkeiten: document.getElementById('unvertraeglichkeiten').value,
      abholzeit: pickupTime,
      gesamtpreis: document.getElementById('totalPrice').innerText
    };

    fetch(scriptURL, {
      method: 'POST',
      mode: 'no-cors',
      cache: 'no-cache',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(formData)
    })
    .then(() => {
        // Erfolgreich abgeschickt -> Formular zurücksetzen
        document.getElementById('orderForm').reset();
        document.getElementById('bowlOptions').style.display = 'none';
        document.querySelectorAll('.option-label').forEach(l => l.classList.remove('selected'));
        calculateTotal();
        
        // Visuelles Bestätigungs-Modal öffnen
        showModal(userName, pickupTime);
    })
    .catch(error => {
      alert("Hoppla! Es gab einen Fehler beim Absenden. Bitte prüfe deine Internetverbindung.");
      console.error('Error!', error);
    })
    .finally(() => {
      // Ladezustand beenden
      submitBtn.disabled = false;
      btnText.innerText = "Kostenpflichtig vorbestellen";
      loadingSpinner.style.display = 'none';
    });
  });
</script>

</body>
</html>
