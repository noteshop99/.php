
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>HTML Website Creator</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body { font-family: sans-serif; background: #f0f0f0; padding: 20px; }
    h1 { text-align: center; }
    input, textarea, button {
      width: 100%; padding: 10px; margin: 10px 0;
      font-size: 16px; border: none; border-radius: 5px;
    }
    button {
      background: #007BFF; color: white; cursor: pointer;
    }
    .link-box, .history-box {
      margin-top: 15px; background: #e0ffe0;
      padding: 10px; border-radius: 5px;
    }
    .history-item {
      margin: 5px 0; background: #fff;
      padding: 8px; border-radius: 5px;
    }
    .sub-buttons { display: none; margin-top: 5px; }
    .modal {
      display: none; position: fixed; top: 0; left: 0;
      width: 100%; height: 100%;
      background: rgba(0,0,0,0.6);
      justify-content: center; align-items: center;
      z-index: 999;
    }
    .modal-content {
      background: white; padding: 20px;
      max-width: 95%; max-height: 90%;
      overflow: auto; border-radius: 10px;
    }
    .modal-content textarea {
      width: 100%; height: 70vh;
      font-family: monospace; font-size: 16px;
    }
    .close-btn {
      background: red; color: white;
      padding: 5px 10px; float: right;
      cursor: pointer; border: none;
    }
    #alertModal .modal-content {
      text-align: center;
    }
  </style>
<link rel="me" href="https://www.blogger.com/profile/17476886733782990816" />
<meta name='google-adsense-platform-account' content='ca-host-pub-1556223355139109'/>
<meta name='google-adsense-platform-domain' content='blogspot.com'/>
</head>
<body>

<h1>Website Creator</h1>

<input type="text" id="siteName" placeholder="Enter website name (example: mysite)">
<input type="password" id="sitePassword" placeholder="Enter password (112211 required)">
<textarea id="htmlCode" rows="10" placeholder="Paste your HTML code here..."></textarea>
<button onclick="createSite()">Create Website</button>
<button onclick="showHistory()">History</button>

<div class="link-box" id="linkBox" style="display:none;">
  <strong>Your site is ready:</strong><br>
  <input type="text" id="siteLink" readonly>
  <button onclick="copyLink()">Copy Link</button>
</div>

<div class="history-box" id="historyBox" style="display:none;">
  <strong>Saved Websites:</strong>
  <div id="historyList"></div>
</div>

<!-- Code View/Edit Modal -->
<div class="modal" id="codeModal">
  <div class="modal-content">
    <button class="close-btn" onclick="closeModal()">Close</button>
    <h3 id="modalTitle">View/Edit Code</h3>
    <textarea id="modalTextarea"></textarea>
    <button onclick="saveEditedCode()">Save Changes</button>
  </div>
</div>

<!-- Custom Alert Modal -->
<div class="modal" id="alertModal">
  <div class="modal-content">
    <button class="close-btn" onclick="closeAlertModal()">×</button>
    <p id="alertMessage"></p>
  </div>
</div>

<script>
  let editingSite = null;

  function createSite() {
    const name = document.getElementById('siteName').value.trim();
    const code = document.getElementById('htmlCode').value;
    const password = document.getElementById('sitePassword').value;
    if (!name || !code || !password) return showAlert('Please enter name, code, and password.');
    if (password !== "112211") return showAlert("Invalid password.");
    if (localStorage.getItem('site-' + name)) return showAlert('This name is already used.');
    localStorage.setItem('site-' + name, code);
    const link = `${location.origin}${location.pathname}?site=${name}.html`;
    document.getElementById('siteLink').value = link;
    document.getElementById('linkBox').style.display = 'block';
  }

  function copyLink() {
    const linkInput = document.getElementById('siteLink');
    linkInput.select(); document.execCommand('copy');
    showAlert('Link copied!');
  }

  function showHistory() {
    const listBox = document.getElementById('historyList');
    listBox.innerHTML = '';
    for (let key in localStorage) {
      if (key.startsWith('site-')) {
        const name = key.replace('site-', '');
        const item = document.createElement('div');
        item.className = 'history-item';
        item.innerHTML = `
          ${name} 
          <button onclick="toggleSub('${name}')">Show HTML</button>
          <div class="sub-buttons" id="sub-${name}">
            <button onclick="viewCode('${name}')">View Code</button>
            <button onclick="viewSite('${name}')">View Website</button>
            <button onclick="copyCode('${name}')">Copy HTML</button>
            <button onclick="editCode('${name}')">Edit HTML</button>
            <button onclick="renameSitePrompt('${name}')">Change Name</button>
          </div>
        `;
        listBox.appendChild(item);
      }
    }
    document.getElementById('historyBox').style.display = 'block';
  }

  function toggleSub(name) {
    const el = document.getElementById('sub-' + name);
    el.style.display = el.style.display === 'block' ? 'none' : 'block';
  }

  function viewCode(name) {
    const code = localStorage.getItem('site-' + name) || 'No code found.';
    document.getElementById('modalTitle').innerText = 'View Code';
    document.getElementById('modalTextarea').value = code;
    document.getElementById('modalTextarea').readOnly = true;
    document.getElementById('codeModal').style.display = 'flex';
  }

  function viewSite(name) {
    const code = localStorage.getItem('site-' + name);
    const newWindow = window.open();
    newWindow.document.write(code);
  }

  function copyCode(name) {
    const code = localStorage.getItem('site-' + name);
    const temp = document.createElement('textarea');
    temp.value = code;
    document.body.appendChild(temp);
    temp.select();
    document.execCommand('copy');
    document.body.removeChild(temp);
    showAlert('Code copied!');
  }

  function editCode(name) {
    editingSite = name;
    const code = localStorage.getItem('site-' + name) || '';
    document.getElementById('modalTitle').innerText = 'Edit Code';
    document.getElementById('modalTextarea').value = code;
    document.getElementById('modalTextarea').readOnly = false;
    document.getElementById('codeModal').style.display = 'flex';
  }

  function saveEditedCode() {
    if (!editingSite) return;
    const newCode = document.getElementById('modalTextarea').value;
    localStorage.setItem('site-' + editingSite, newCode);
    showAlert('Code updated!');
    closeModal();
  }

  function renameSitePrompt(oldName) {
    const newName = prompt("Enter New Website Name:");
    if (!newName) return;
    if (localStorage.getItem('site-' + newName)) {
      alert("This name is already used.");
      return;
    }

    const code = localStorage.getItem('site-' + oldName);
    if (!code) return alert("Original website not found.");

    localStorage.setItem('site-' + newName, code);
    localStorage.removeItem('site-' + oldName);

    const link = `${location.origin}${location.pathname}?site=${newName}.html`;
    document.getElementById('siteLink').value = link;
    document.getElementById('linkBox').style.display = 'block';
    alert("Name changed successfully!");
  }

  function closeModal() {
    editingSite = null;
    document.getElementById('codeModal').style.display = 'none';
  }

  function showAlert(message) {
    document.getElementById('alertMessage').innerText = message;
    document.getElementById('alertModal').style.display = 'flex';
  }

  function closeAlertModal() {
    document.getElementById('alertModal').style.display = 'none';
  }

  // Auto redirect if site param exists
  const params = new URLSearchParams(window.location.search);
  const site = params.get("site");
  if (site) {
    const nameOnly = site.replace('.html', '');
    const html = localStorage.getItem("site-" + nameOnly);
    if (html) {
      document.head.innerHTML = `<title>${(html.match(/<title>(.*?)<\/title>/i) || [])[1] || 'User Site'}</title>`;
      document.body.innerHTML = '';
      document.write(html);
    } else {
      document.write("<style>body{margin:0;display:flex;justify-content:center;align-items:center;height:100vh;background:#f0f0f0}</style><img src='https://i.postimg.cc/63FYtGY6/IMG-20250421-230021.jpg' style='max-width:100%;max-height:100%' alt='Not Found'>");
    }
    throw new Error("Stop further loading");
  }
</script>
</body>
</html>
