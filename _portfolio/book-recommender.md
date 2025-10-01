---
title: "Book Recommendation Web App"
excerpt: "A GitHub Pages frontend connected to a Flask backend on PythonAnywhere.<br/><img src='/images/bookrec.png'>"
collection: portfolio
---

Interactive forms can be powerful tools to connect static websites with backend intelligence.  
This project demonstrates how **GitHub Pages** (static frontend) can work together with a **Flask API** hosted on **PythonAnywhere** to provide live book recommendations.

## 🚀 Try it here

<form id="book-form" method="POST">
  <label for="book1">Book 1:</label>
  <input type="text" id="book1" name="field_1" autocomplete="off"><br><br>

  <label for="book2">Book 2:</label>
  <input type="text" id="book2" name="field_2" autocomplete="off"><br><br>

  <label for="book3">Book 3:</label>
  <input type="text" id="book3" name="field_3" autocomplete="off"><br><br>

  <button type="submit">Get Recommendation</button>
</form>

<div id="result"></div>

<script>
async function fetchSuggestions(query) {
  const response = await fetch(`https://miterdemes.pythonanywhere.com/suggest?q=${encodeURIComponent(query)}`);
  return await response.json();
}

function setupAutocomplete(input) {
  const suggestionBox = document.createElement("div");
  suggestionBox.style.border = "1px solid #ccc";
  suggestionBox.style.position = "absolute";
  suggestionBox.style.backgroundColor = "#fff";
  suggestionBox.style.zIndex = "1000";
  suggestionBox.style.maxHeight = "150px";
  suggestionBox.style.overflowY = "auto";
  suggestionBox.style.width = input.offsetWidth + "px";
  suggestionBox.style.display = "none";

  // Pozíció beállítása közvetlenül az input alatt
  suggestionBox.style.left = input.offsetLeft + "px";
  suggestionBox.style.top = (input.offsetTop + input.offsetHeight) + "px";

  // A suggestionBox az input szülőjéhez kerül
  input.parentNode.appendChild(suggestionBox);

  input.addEventListener("input", async () => {
    const query = input.value;
    if (!query) {
      suggestionBox.style.display = "none";
      return;
    }

    const suggestions = await fetchSuggestions(query);
    suggestionBox.innerHTML = "";
    suggestions.forEach(suggestion => {
      const item = document.createElement("div");
      item.textContent = suggestion;
      item.style.padding = "5px";
      item.style.cursor = "pointer";
      item.addEventListener("click", () => {
        input.value = suggestion;
        suggestionBox.style.display = "none";
      });
      suggestionBox.appendChild(item);
    });

    suggestionBox.style.display = suggestions.length ? "block" : "none";
  });

  document.addEventListener("click", (e) => {
    if (!suggestionBox.contains(e.target) && e.target !== input) {
      suggestionBox.style.display = "none";
    }
  });
}

["book1", "book2", "book3"].forEach(id => {
  setupAutocomplete(document.getElementById(id));
});

document.getElementById("book-form").addEventListener("submit", async function(e) {
  e.preventDefault();

  const formData = new FormData(this);
  const response = await fetch("https://miterdemes.pythonanywhere.com/process", {
    method: "POST",
    body: formData
  });

  const data = await response.json();
  document.getElementById("result").innerText = data.message;
});
</script>

## 🔧 How It Works
1. User enters up to three favorite books in the form above.  
2. The form POSTs the data to a **Flask API** hosted on PythonAnywhere.  
3. The backend selects a candidate book and asks the **OpenAI API** for a short description.  
4. The recommendation is displayed directly in the browser.

## 📷 Demo
<br/><img src='/images/bookrec.png'>

## 🔮 Future Improvements
- Replace simple logic with a **content-based recommender system**.  
- Log user interactions in a **database** for analysis.  
- Expand to recommend **movies, concerts, or cultural events**.  
