<!DOCTYPE html>
<html>
<head>
  <title>Free HTML Website Builder</title>
  <style>
    body { font-family: sans-serif; padding: 20px; }
    input, textarea, button { width: 100%; margin-top: 10px; padding: 10px; }
    .link-box { margin-top: 20px; background: #f0f0f0; padding: 10px; }
  </style>
</head>
<body>

  <h2>Create Your Website</h2>
  <input type="text" id="siteName" placeholder="Enter Website Name (no space)">
  <textarea id="siteCode" rows="10" placeholder="Enter HTML Code Here..."></textarea>
  <button onclick="createSite()">Create Website</button>

  <div class="link-box" id="output"></div>

  <script>
    async function createSite() {
      const name = document.getElementById("siteName").value.trim();
      const code = document.getElementById("siteCode").value.trim();

      if (!name || !code) return alert("Fill all fields");

      const blob = new Blob([code], { type: 'text/html' });
      const url = URL.createObjectURL(blob);

      const anchor = document.createElement("a");
      anchor.href = url;
      anchor.download = name + ".html";
      anchor.click();

      document.getElementById("output").innerHTML = `
        <p>Your HTML file downloaded.</p>
        <p>Upload this to your <b>GitHub repo</b> at: <code>${name}.html</code></p>
        <p>Then your site will be live at:</p>
        <a href="https://YOUR_GITHUB_USERNAME.github.io/${name}.html" target="_blank">
          https://YOUR_GITHUB_USERNAME.github.io/${name}.html
        </a>
      `;
    }
  </script>
</body>
</html>
