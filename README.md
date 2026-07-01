<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Instant YouTube Previewer</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f0f2f5;
            margin: 0;
            padding: 40px 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        h1 {
            color: #1a1a1a;
            margin-bottom: 10px;
        }
        p {
            color: #666;
            margin-bottom: 30px;
            text-align: center;
        }
        .search-container {
            width: 100%;
            max-width: 600px;
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
        }
        input[type="text"] {
            flex: 1;
            padding: 12px 15px;
            border: 2px solid #ccc;
            border-radius: 6px;
            font-size: 16px;
            outline: none;
            transition: border-color 0.2s;
        }
        input[type="text"]:focus {
            border-color: #ff0000; /* YouTube Red */
        }
        button {
            padding: 12px 24px;
            background-color: #ff0000;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: background 0.2s;
        }
        button:hover {
            background-color: #cc0000;
        }
        .preview-box {
            width: 100%;
            max-width: 800px;
            background: white;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
            overflow: hidden;
            display: none; /* Hidden until a video is loaded */
        }
        .video-wrapper {
            position: relative;
            padding-bottom: 56.25%; /* 16:9 Aspect Ratio */
            height: 0;
        }
        .video-wrapper iframe {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border: 0;
        }
        .error-message {
            color: #ff0000;
            font-weight: bold;
            display: none;
            margin-bottom: 20px;
        }
    </style>
</head>
<body>

    <h1>Instant YouTube Previewer</h1>
    <p>Paste any YouTube watch link, share link, or Shorts URL below to generate an instant embed preview.</p>

    <div class="search-container">
        <input type="text" id="youtubeUrl" placeholder="Paste YouTube link here... (e.g., https://www.youtube.com/watch?v=...)">
        <button onclick="loadVideo()">Preview</button>
    </div>

    <div class="error-message" id="errorMsg">Invalid YouTube URL. Please try again!</div>

    <div class="preview-box" id="previewBox">
        <div class="video-wrapper">
            <iframe id="videoPlayer" src="" allowfullscreen allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"></iframe>
        </div>
    </div>

    <script>
        function extractVideoId(url) {
            // Regular expression to handle standard links, share links (youtu.be), and shorts
            const regExp = /^.*(youtu.be\/|v\/|u\/\w\/|embed\/|watch\?v=|\&v=|shorts\/)([^#\&\?]*).*/;
            const match = url.match(regExp);
            
            return (match && match[2].length === 11) ? match[2] : null;
        }

        function loadVideo() {
            const urlInput = document.getElementById('youtubeUrl').value.trim();
            const videoId = extractVideoId(urlInput);
            const previewBox = document.getElementById('previewBox');
            const videoPlayer = document.getElementById('videoPlayer');
            const errorMsg = document.getElementById('errorMsg');

            if (videoId) {
                // Hide error if it was showing
                errorMsg.style.display = 'none';
                
                // Construct the secure embed URL and update the iframe source
                videoPlayer.src = `https://www.youtube.com/embed/${videoId}`;
                
                // Slide/Show the video box container
                previewBox.style.display = 'block';
            } else {
                // Hide player and show error if link is broken
                previewBox.style.display = 'none';
                videoPlayer.src = '';
                errorMsg.style.display = 'block';
            }
        }

        // Allows hitting the "Enter" key to load the video automatically
        document.getElementById('youtubeUrl').addEventListener('keypress', function (e) {
            if (e.key === 'Enter') {
                loadVideo();
            }
        });
    </script>

</body>
</html>
# Previewer
