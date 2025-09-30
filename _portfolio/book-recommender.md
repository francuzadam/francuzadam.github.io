---
title: "Book Recommendation Web App"
excerpt: "A GitHub Pages frontend connected to a Flask backend on PythonAnywhere.<br/><img src='/images/bookrec.png'>"
collection: portfolio
---

Interactive forms can be powerful tools to connect static websites with backend intelligence.  
This project demonstrates how **GitHub Pages** (static frontend) can work together with a **Flask API** hosted on **PythonAnywhere** to provide live book recommendations.

## 🚀 Try it here

<form method="POST" action="https://miterdemes.pythonanywhere.com/process">
  <label for="book1">Book 1:</label>
  <input type="text" id="book1" name="book1"><br><br>

  <label for="book2">Book 2:</label>
  <input type="text" id="book2" name="book2"><br><br>

  <label for="book3">Book 3:</label>
  <input type="text" id="book3" name="book3"><br><br>

  <button type="submit">Get Recommendation</button>
</form>

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