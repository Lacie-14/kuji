<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>くじ引きアプリ</title>
  <style>
    body {
      font-family: sans-serif;
      margin: 0;
      padding: 10px;
      background: #f5f5f5;
      box-sizing: border-box;
    }
    h1 {
      font-size: 20px;
      text-align: center;
      margin-top: 10px;
    }
    textarea {
      width: 100%;
      height: 200px;
      font-size: 14px;
      padding: 8px;
      box-sizing: border-box;
      resize: none;
    }
    .corner-buttons {
      display: flex;
      justify-content: space-between;
      margin-bottom: 10px;
    }
    .corner-buttons button {
      font-size: 12px;
      padding: 6px 10px;
    }
    .draw-section {
      text-align: center;
      margin-top: 20px;
    }
    .draw-button {
      font-size: 24px;
      padding: 10px 30px;
      margin-top: 10px;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 5px;
    }
    .drawn-result {
      font-size: 20px;
      margin-top: 20px;
      color: #d32f2f;
      font-weight: bold;
    }
    .dotted {
      border-top: 1px dotted #aaa;
      margin: 5px 0;
    }
    .preview, .history {
      font-size: 14px;
      background: white;
      padding: 10px;
      margin-top: 10px;
      border-radius: 4px;
    }
    .hidden {
      display: none;
    }
  </style>
</head>
<body>

  <h1>くじ引きアプリ</h1>

  <div class="corner-buttons">
    <div>
      <button onclick="loadData()">読み込み</button>
      <button onclick="saveData()">保存</button>
      <button onclick="deleteData()">削除</button>
    </div>
    <button onclick="togglePreview()">プレビュー</button>
  </div>

  <textarea id="input" placeholder="ここに最大30個の候補を入力してください（1行に1つ）"></textarea>
  <button onclick="confirmEntries()" style="margin-top: 10px;">確認</button>

  <div id="drawSection" class="draw-section hidden">
    <div id="drawnResult" class="drawn-result"></div>
    <button class="draw-button" onclick="draw()">くじを引く</button>
    <div class="corner-buttons" style="justify-content: center; margin-top: 10px;">
      <button onclick="goBack()">戻る</button>
    </div>
  </div>

  <div id="previewList" class="preview hidden"></div>
  <div id="historyList" class="history hidden"></div>

  <script>
    let entries = [];
    let drawHistory = [];

    function confirmEntries() {
      const raw = document.getElementById('input').value.trim().split('\n').filter(e => e.trim());
      entries = raw.slice(0, 30);
      if (entries.length === 0) {
        alert("候補がありません。");
        return;
      }
      drawHistory = [];
      document.getElementById('input').classList.add('hidden');
      document.querySelector('button[onclick="confirmEntries()"]').classList.add('hidden');
      document.getElementById('drawSection').classList.remove('hidden');
      document.getElementById('historyList').classList.remove('hidden');
      updateHistory();
    }

    function goBack() {
      document.getElementById('input').classList.remove('hidden');
      document.querySelector('button[onclick="confirmEntries()"]').classList.remove('hidden');
      document.getElementById('drawSection').classList.add('hidden');
      document.getElementById('historyList').classList.add('hidden');
    }

    function draw() {
      if (entries.length === 0) return;
      const picked = entries[Math.floor(Math.random() * entries.length)];
      drawHistory.push(picked);
      document.getElementById('drawnResult').textContent = "結果: " + picked;
      updateHistory();
    }

    function updateHistory() {
      const historyEl = document.getElementById('historyList');
      historyEl.innerHTML = "<strong>履歴:</strong><br>" + drawHistory.map((e, i) => `${i + 1}. ${e}`).join("<br>");
    }

    function saveData() {
      const raw = document.getElementById('input').value.trim().split('\n').filter(e => e.trim());
      localStorage.setItem('entries', JSON.stringify(raw.slice(0, 30)));
      localStorage.setItem('history', JSON.stringify(drawHistory));
      alert("保存しました！");
    }

    function loadData() {
      const saved = localStorage.getItem('entries');
      if (saved) {
        entries = JSON.parse(saved);
        document.getElementById('input').value = entries.join('\n');
      }
      const savedHistory = localStorage.getItem('history');
      if (savedHistory) {
        drawHistory = JSON.parse(savedHistory);
        updateHistory();
        document.getElementById('historyList').classList.remove('hidden');
      }
      alert("読み込みました！");
    }

    function deleteData() {
      if (confirm("本当に削除しますか？")) {
        localStorage.removeItem('entries');
        localStorage.removeItem('history');
        document.getElementById('input').value = '';
        entries = [];
        drawHistory = [];
        document.getElementById('drawnResult').textContent = '';
        document.getElementById('historyList').innerHTML = '';
        alert("削除しました！");
      }
    }

    function togglePreview() {
      const preview = document.getElementById('previewList');
      if (preview.classList.contains('hidden')) {
        const list = document.getElementById('input').value.trim().split('\n').filter(e => e.trim());
        preview.innerHTML = "<strong>プレビュー:</strong><br>" + list.map((e, i) => `${i + 1}. ${e} <div class='dotted'></div>`).join("");
        preview.classList.remove('hidden');
      } else {
        preview.classList.add('hidden');
      }
    }
  </script>
</body>
</html>
