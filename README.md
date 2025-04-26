<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Raj Text Repeater</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    html, body {
      height: 100%;
      width: 100%;
      font-family: 'Segoe UI', sans-serif;
      background: radial-gradient(circle at center, #0f0f0f, #000000);
      color: #fff;
      overflow: auto;
    }

    body {
      display: flex;
      justify-content: center;
      align-items: start;
      padding: 50px 20px;
    }

    .container {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 20px;
      padding: 40px 30px;
      box-shadow: 0 0 40px rgba(0, 255, 255, 0.2);
      backdrop-filter: blur(10px);
      border: 2px solid rgba(0, 255, 255, 0.3);
      position: relative;
      width: 100%;
      max-width: 500px;
      text-align: center;
      z-index: 1;
    }

    .container::before {
      content: "";
      position: absolute;
      width: 300px;
      height: 300px;
      background: conic-gradient(from 0deg, cyan, blue, cyan);
      border-radius: 50%;
      animation: rotate 5s linear infinite;
      top: -120px;
      left: -120px;
      z-index: 0;
      filter: blur(60px);
      opacity: 0.4;
    }

    @keyframes rotate {
      from { transform: rotate(0deg); }
      to { transform: rotate(360deg); }
    }

    h1 {
      font-size: 2rem;
      margin-bottom: 25px;
      position: relative;
      z-index: 1;
    }

    input, button {
      padding: 12px 16px;
      margin: 8px 5px;
      font-size: 1rem;
      border: none;
      border-radius: 10px;
      outline: none;
      z-index: 1;
      position: relative;
    }

    input {
      background: #222;
      color: #0ff;
      border: 1px solid #0ff;
      width: 80%;
      margin-bottom: 10px;
    }

    button {
      background: linear-gradient(45deg, #00ffff, #0077ff);
      color: black;
      font-weight: bold;
      cursor: pointer;
      transition: transform 0.2s ease, box-shadow 0.3s ease;
    }

    button:hover {
      transform: scale(1.05);
      box-shadow: 0 0 15px #00ffff;
    }

    #output {
      margin-top: 20px;
      padding: 15px;
      background: rgba(0, 255, 255, 0.1);
      border: 1px solid #0ff;
      border-radius: 10px;
      white-space: pre-line;
      color: #0ff;
      font-family: monospace;
      min-height: 100px;
      max-height: 300px;
      overflow-y: auto;
      width: 100%;
      position: relative;
      z-index: 1;
    }

    #copyBtn {
      display: none;
      margin-top: 10px;
    }

    @media screen and (max-width: 480px) {
      h1 {
        font-size: 1.5rem;
      }

      input {
        width: 100%;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Raj Text Repeater</h1>
    <input type="text" id="textInput" placeholder="Enter your text">
    <br>
    <input type="number" id="repeatCount" placeholder="Repeat count" min="1">
    <br>
    <button onclick="repeatText()">Repeat</button>
    <button id="copyBtn" onclick="copyText()">Copy</button>
    <div id="output"></div>
  </div>

  <script>
    function repeatText() {
      const text = document.getElementById('textInput').value;
      const count = parseInt(document.getElementById('repeatCount').value);
      let output = '';

      if (!text || isNaN(count) || count < 1) {
        output = 'Please enter valid text and number.';
        document.getElementById('copyBtn').style.display = 'none';
      } else {
        for (let i = 0; i < count; i++) {
          output += text + '\n';
        }
        document.getElementById('copyBtn').style.display = 'inline-block';
      }

      document.getElementById('output').innerText = output.trim();
    }

    function copyText() {
      const outputText = document.getElementById('output').innerText;
      navigator.clipboard.writeText(outputText).then(() => {
        alert('Text copied to clipboard!');
      }).catch(err => {
        alert('Failed to copy text: ' + err);
      });
    }
  </script>
</body>
</html>
