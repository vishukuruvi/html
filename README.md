<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Search Input History</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 20px;
        }
        input {
            padding: 8px;
            font-size: 16px;
            width: 60%;
            margin-bottom: 10px;
        }
        button {
            padding: 10px 15px;
            font-size: 16px;
            margin: 10px;
            cursor: pointer;
        }
        ul {
            list-style: none;
            padding: 0;
        }
        li {
            margin: 5px 0;
            font-size: 16px;
        }
    </style>
</head>
<body>

    <h2>Search Input History (Last 10 Minutes)</h2>
    
    <input type="text" id="searchInput" placeholder="Enter text...">
    <button onclick="storeSearch()">Save Search</button>
    <button onclick="showSearchHistory()">Show History</button>

    <h3>Search History:</h3>
    <ul id="searchHistoryList"></ul>

    <script>
        function storeSearch() {
            let inputText = document.getElementById("searchInput").value.trim();
            if (!inputText) {
                alert("Please enter some text.");
                return;
            }

            let timestamp = new Date().getTime(); // Current time

            // Retrieve existing history or initialize
            let history = JSON.parse(localStorage.getItem("searchHistory")) || [];

            // Add new search entry
            history.push({ text: inputText, time: timestamp });

            // Save to localStorage
            localStorage.setItem("searchHistory", JSON.stringify(history));

            // Clear input field
            document.getElementById("searchInput").value = "";

            alert("Search saved! It will be stored for 10 minutes.");
        }

        function showSearchHistory() {
            let history = JSON.parse(localStorage.getItem("searchHistory")) || [];
            let historyList = document.getElementById("searchHistoryList");
            historyList.innerHTML = "";

            let currentTime = new Date().getTime();
            let tenMinutesAgo = currentTime - 10 * 60 * 1000; // 10 minutes in milliseconds

            // Filter out searches older than 10 minutes
            let filteredHistory = history.filter(entry => entry.time >= tenMinutesAgo);

            if (filteredHistory.length === 0) {
                historyList.innerHTML = "<li>No recent searches</li>";
                return;
            }

            filteredHistory.forEach(entry => {
                let li = document.createElement("li");
                li.textContent = entry.text;
                historyList.appendChild(li);
            });

            // Save only recent history (remove old data)
            localStorage.setItem("searchHistory", JSON.stringify(filteredHistory));
        }
    </script>

</body>
</html>
