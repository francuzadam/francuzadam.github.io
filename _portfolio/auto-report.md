---
title: "Travel recommendations with AI"
excerpt: "Based on some preferences, AI suggests travel recommendations"
collection: portfolio
---

<p>This project uses a GitHub form to collect the required data. Then, a webhook sends this information to the n8n AI automation tool. The OpenAI agent generates recommendations in JSON format, which are then sent back via the webhook.</p>


<form id="travelForm">
  <label for="travelType">Type of Travel:</label>
  <select id="travelType" name="travelType" required>
    <option value="beach">Beach</option>
    <option value="sightseeing">Sightseeing</option>
    <option value="nature">Nature</option>
    <option value="adventure">Adventure</option>
  </select>

  <label for="budget">Budget:</label>
  <select id="budget" name="budget" required>
    <option value="low">Low</option>
    <option value="medium">Medium</option>
    <option value="high">High</option>
  </select>

  <label for="duration">Duration:</label>
  <select id="duration" name="duration" required>
    <option value="3 days">3 days</option>
    <option value="1 week">1 week</option>
    <option value="2 weeks">2 weeks</option>
  </select>

  <label for="location">Departure Location:</label>
  <input type="text" id="location" name="location" placeholder="e.g. Budapest" required>

  <label for="season">Season or Month:</label>
  <input type="text" id="season" name="season" placeholder="e.g. October" required>

  <label for="notes">Notes / Personal Preferences:</label>
  <textarea id="notes" name="notes" rows="3" placeholder="e.g. not too hot, lots of sights to see..."></textarea>

  <button type="submit">Request Recommendation</button>
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

  try {
    const response = await fetch("https://fradam99.app.n8n.cloud/webhook/0804ce0e-0240-40a0-9752-874be5147124", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(data)
    });

    if (response.ok) {
      const result = await response.json();
      console.log("Teljes válasz JSON:", result);

      document.getElementById("responseMessage").innerText = "Ajánlott úti célok:";
      const container = document.getElementById("recommendations");
      container.innerHTML = "";

      result.forEach(rec => {
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
  } catch (error) {
    console.error("Hiba a kérés során:", error);
    document.getElementById("responseMessage").innerText = "Technikai hiba történt.";
  }
});
