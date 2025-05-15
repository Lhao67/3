<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cookie to JSON</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            line-height: 1.6;
            color: #333;
        }
        h1 {
            color: #000;
            font-size: 2em;
            margin-bottom: 0.5em;
        }
        textarea {
            width: 100%;
            height: 120px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-family: monospace;
            margin-bottom: 10px;
        }
        button {
            background-color: #0366d6;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
            margin-right: 10px;
        }
        button:hover {
            background-color: #0356b6;
        }
        pre {
            background: #f6f8fa;
            padding: 16px;
            border-radius: 4px;
            overflow-x: auto;
            border: 1px solid #ddd;
        }
        .export-buttons {
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h1>Cookie to JSON</h1>
    <textarea id="cookieInput" placeholder="Paste your cookie string here (e.g., name=value; name2=value2)"></textarea>
    <div>
        <button onclick="convert()">Convert</button>
        <div class="export-buttons">
            <button onclick="exportJSON()">Export JSON</button>
            <button onclick="exportText()">Export Text</button>
        </div>
    </div>
    <pre id="output">{}</pre>

    <script>
        function convert() {
            const cookieStr = document.getElementById('cookieInput').value.trim();
            const result = [];
            
            // 严格按原站格式解析
            cookieStr.split(';').forEach(pair => {
                const idx = pair.indexOf('=');
                if (idx >= 0) {
                    const name = pair.substring(0, idx).trim();
                    const value = pair.substring(idx + 1).trim();
                    if (name) {
                        result.push({
                            "domain": ".example.com",  // 默认域名（可修改）
                            "expiry": 20000000000,     // 默认过期时间
                            "httpOnly": false,
                            "name": name,
                            "path": "/",
                            "sameSite": "Lax",
                            "secure": false,
                            "value": value
                        });
                    }
                }
            });

            document.getElementById('output').textContent = JSON.stringify(result, null, 4);
        }

        function exportJSON() {
            const data = document.getElementById('output').textContent;
            const blob = new Blob([data], { type: 'application/json' });
            downloadFile(blob, 'cookies.json');
        }

        function exportText() {
            const data = document.getElementById('output').textContent;
            const blob = new Blob([data], { type: 'text/plain' });
            downloadFile(blob, 'cookies.txt');
        }

        function downloadFile(blob, filename) {
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = filename;
            a.click();
            URL.revokeObjectURL(url);
        }
    </script>
</body>
</html>
