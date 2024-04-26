<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MP4 to MP3 Converter</title>
</head>
<body>
    <h1>MP4 to MP3 Converter</h1>
    <input type="file" accept="video/mp4" id="videoInput">
    <button onclick="convertToMP3()">Convert to MP3</button>
    <a id="downloadLink" style="display: none;" download="output.mp3">Download MP3</a>

    <script>
        function convertToMP3() {
            const inputElement = document.getElementById('videoInput');
            const file = inputElement.files[0];
            if (!file) {
                alert('Please select a video file.');
                return;
            }

            const videoURL = URL.createObjectURL(file);
            const videoElement = document.createElement('video');
            videoElement.src = videoURL;
            videoElement.style.display = 'none';
            document.body.appendChild(videoElement);

            videoElement.onloadeddata = () => {
                const canvas = document.createElement('canvas');
                canvas.width = videoElement.videoWidth;
                canvas.height = videoElement.videoHeight;
                const context = canvas.getContext('2d');
                context.drawImage(videoElement, 0, 0, canvas.width, canvas.height);

                canvas.toBlob(blob => {
                    const url = URL.createObjectURL(blob);
                    const audio = new Audio(url);
                    audio.controls = true;
                    document.body.appendChild(audio);

                    const downloadLink = document.getElementById('downloadLink');
                    downloadLink.href = url;
                    downloadLink.style.display = 'block';
                }, 'audio/mp3');
            };

            videoElement.onerror = () => {
                alert('Error loading the video file.');
            };
        }
    </script>
</body>
</html>
