# webfeed.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Web Feed</title>
    <style>
        body { 
            font-family: Arial; 
            text-align: center; 
            max-width: 800px;
            margin: 0 auto;
        }
        h1 { text-decoration: underline; }
        .item { 
            padding: 10px; 
            border-bottom: 1px solid #ddd; 
            text-align: left; 
        }
    </style>
</head>
<body>
    <h1>Web Feed</h1>
    <div id="feed">Loading...</div>

    <script>
        // Popular news feeds
        const feeds = [
            "https://feeds.bbci.co.uk/news/rss.xml",
            "http://rss.cnn.com/rss/edition.rss",
            "https://rss.nytimes.com/services/xml/rss/nyt/HomePage.xml"
        ];

        // Get and display feed content
        async function loadFeeds() {
            let content = [];
            
            for (let feed of feeds) {
                try {
                    // Use RSS to JSON API
                    const url = `https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(feed)}`;
                    const response = await fetch(url);
                    const data = await response.json();
                    
                    // Get first 3 items from each feed
                    content.push(...(data.items || []).slice(0, 3));
                } catch (err) {
                    console.error("Failed to load:", feed);
                }
            }
            
            // Show the feed items on the page
            document.getElementById("feed").innerHTML = content.map(item => `
                <div class="item">
                    <h3><a href="${item.link}" target="_blank">${item.title}</a></h3>
                    <p>${item.description || 'No description available'}</p>
                </div>
            `).join('');
        }
        
        // Start the process
        loadFeeds();
    </script>
</body>
</html>
