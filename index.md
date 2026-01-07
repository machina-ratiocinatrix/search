---
layout: home
title: Search
---

# Search

<div id="search-results"></div>

<script>
  // Inject Jekyll data into JavaScript
  const utilities = {{ site.data.utilities | jsonify }};
  const baseUrl = "{{ site.baseurl }}";

  document.addEventListener('DOMContentLoaded', function() {
    const urlParams = new URLSearchParams(window.location.search);
    const query = urlParams.get('q');
    const resultsContainer = document.getElementById('search-results');

    if (query) {
      const lowerCaseQuery = query.toLowerCase().trim();
      
      // Filter utilities based on the query verb
      const matchingUtilities = utilities.filter(item => {
        return item.verbs && item.verbs.some(verb => verb.toLowerCase() === lowerCaseQuery);
      });

      displayResults(query, matchingUtilities);
    } else {
      resultsContainer.innerHTML = '<p>No search query provided.</p>';
    }

    function displayResults(searchQuery, results) {
      if (results.length === 0) {
        resultsContainer.innerHTML = '<p>No results found for: <strong>' + escapeHtml(searchQuery) + '</strong></p>';
        return;
      }

      let html = '<h2>Results for: ' + escapeHtml(searchQuery) + '</h2><ul style="list-style-type: none; padding: 0;">';
      
      results.forEach(function(item) {
        const label = item.description ? item.description : item.url;
        // Prepend baseurl to the icon path
        const iconPath = baseUrl + '/assets/icons/' + item.icon;
        const iconHtml = item.icon ? '<img src="' + iconPath + '" alt="" style="width:48px;height:48px;vertical-align:middle;margin-right:10px;">' : '';
        const verbsHtml = item.verbs ? '<br><small>Verbs: ' + item.verbs.join(', ') + '</small>' : '';
        
        html += '<li style="margin-bottom: 20px;">' + 
                  '<a href="' + item.url + '" style="text-decoration: none; display: flex; align-items: center;">' + 
                    iconHtml + 
                    '<div>' +
                      '<strong>' + label + '</strong>' +
                      verbsHtml +
                    '</div>' +
                  '</a>' + 
                '</li>';
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
  });
</script>
