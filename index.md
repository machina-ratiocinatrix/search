---
layout: home
title: Search
---

# Search

<div id="search-results"></div>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    const urlParams = new URLSearchParams(window.location.search);
    const query = urlParams.get('q');
    const resultsContainer = document.getElementById('search-results');

    if (query) {
      if (true) {
        displayDefaultResults(query);
      }
    } else {
      resultsContainer.innerHTML = '<p>No search query provided.</p>';
    }

    function displayDefaultResults(searchQuery) {
      const exampleUrls = [
        "https://developer.android.com",
        "https://github.com",
        "https://stackoverflow.com"
      ];

      let html = '<h2>Results for: ' + escapeHtml(searchQuery) + '</h2><ul>';
      
      exampleUrls.forEach(function(url) {
        html += '<li><a href="' + url + '">' + url + '</a></li>';
      });
      
      html += '</ul>';
      resultsContainer.innerHTML = html;
    }

    // Basic HTML escaping to prevent XSS
    function escapeHtml(text) {
      if (!text) return text;
      return text
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#039;");
    }
  }); // Missing closing brace for the DOMContentLoaded event listener
</script>
