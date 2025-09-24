---
title: "Automatikus riport igénylés"
excerpt: "Egyszerű űrlap, amely emailben küld automatikus riportot.<br/><img src='/images/g"
collection: portfolio
---

Ez egy automatikus riportgeneráló űrlap. Add meg az email címed, és elküldjük neked a riportot!

<h2>Automatikus riport igénylése</h2>
<p>Kérlek, add meg az email címed, és elküldjük neked az automatikusan generált riportot!</p>

<form id="reportForm">
  <label for="email">Email cím:</label><br>
  <input type="email" id="email" name="email" required><br><br>
  <button type="submit">Riport kérése</button>
</form>

<div id="responseMessage"></div>

<script>
document.getElementById("reportForm").addEventListener("submit", async function(e) {
  e.preventDefault();
  const email = document.getElementById("email").value;

  const response = await fetch("https://n8n.yourdomain.com/webhook/auto-report", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ email: email })
  });

  if (response.ok) {
    document.getElementById("responseMessage").innerText = "Köszönjük! A riport hamarosan megérkezik az email címedre.";
  } else {
    document.getElementById("responseMessage").innerText = "Hiba történt a kérés feldolgozása során.";
  }
});
</script>
