---
title: "Travel recommendations with AI"
excerpt: "Teszt teszt"
collection: portfolio
---

<h2>Utazási tippek AI segítségével</h2>
<p>Kérlek, add meg az email címed, és elküldjük neked az automatikusan generált riportot!</p>

<form id="travelForm">
  <label for="travelType">Utazás típusa:</label><br>
  <select id="travelType" name="travelType" required>
    <option value="tengerpart">Tengerpart</option>
    <option value="városnézés">Városnézés</option>
    <option value="természet">Természet</option>
    <option value="kaland">Kaland</option>
  </select><br><br>

  <label for="budget">Költségkeret:</label><br>
  <select id="budget" name="budget" required>
    <option value="alacsony">Alacsony</option>
    <option value="közepes">Közepes</option>
    <option value="magas">Magas</option>
  </select><br><br>

  <label for="duration">Időtartam:</label><br>
  <select id="duration" name="duration" required>
    <option value="3 nap">3 nap</option>
    <option value="1 hét">1 hét</option>
    <option value="2 hét">2 hét</option>
  </select><br><br>

  <label for="location">Indulási hely:</label><br>
  <input type="text" id="location" name="location" placeholder="Pl. Budapest" required><br><br>

  <label for="season">Évszak vagy hónap:</label><br>
  <input type="text" id="season" name="season" placeholder="Pl. október" required><br><br>

  <label for="notes">Megjegyzés / egyéni preferencia:</label><br>
  <textarea id="notes" name="notes" rows="3" placeholder="Pl. ne legyen túl meleg, legyen sok látnivaló..."></textarea><br><br>

  <button type="submit">Ajánlás kérése</button>
</form>


<div id="responseMessage"></div>
<div id="recommendations"></div>

<script>
document.getElementById("travelForm").addEventListener("submit", async function(e) {
  e.preventDefault();

  const data = {
    travelType: document.getElementById("travelType").value,
    budget: document.getElementById("budget").value,
    duration: document.getElementById("duration").value,
    location: document.getElementById("location").value,
    season: document.getElementById("season").value,
    notes: document.getElementById("notes").value
  };

  const response = await fetch("https://fradam99.app.n8n.cloud/webhook/0804ce0e-0240-40a0-9752-874be5147124", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(data)
  });

  if (response.ok) {
    const result = await response.json();
    document.getElementById("responseMessage").innerText = "Ajánlott úti célok:";

    const container = document.getElementById("recommendations");
    container.innerHTML = "";

    const recommendations = result[0]?.message?.content?.recommendations || [];

    recommendations.recommendations.forEach(rec => {
      const card = document.createElement("div");
      card.style.marginBottom = "15px";

      card.innerHTML = `
        <strong>${rec.label}</strong><br>
        <span>${rec.description}</span>
      `;
      container.appendChild(card);
    });
  } else {
    document.getElementById("responseMessage").innerText = "Hiba történt a kérés feldolgozása során.";
  }
});
</script>
