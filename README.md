<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>くじ引き</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      max-width: 360px;
      margin: auto;
      padding: 10px;
      background-color: #f4f9ff;
      position: relative;
    }
    #inputArea {
      width: 100%;
      height: 200px;
      padding: 6px;
      font-size: 13px;
      box-sizing: border-box;
      border: 1px dashed #ccc;
      background-color: #ffffff;
      white-space: pre-wrap;
      line-height: 1.4;
    }
    .corner-buttons {
      position: absolute;
      top: 5px;
      display: flex;
      justify-content: space-between;
      width: 100%;
      padding: 0 5px;
      z-index: 1;
    }
    .corner-buttons button {
      font-size: 11px;
      padding: 3px 6px;
      margin-left: 4px;
    }
    #drawSection {
      margin-top: 20px;
      display: none;
      text-align: center;
    }
    #drawButton {
      font-size: 18px;
      padding: 12px 24px;
      margin-top: 12px;
      background-color: #007acc;
      color: white;
      border: none;
      border-radius: 6px;
    }
    #result {
      font-size: 18px;
      font-weight: bold;
      color: #007acc;
      margin-bottom: 8px;
    }
    #history {
      font-size: 12px;
      margin-top: 15px;
      color: #333;
      border-top: 1px dashed #aaa;
      padding-top: 8px;
      text-align: left;
    }
    #backButton {
      display: none;
      font-size: 11px;
      padding: 4px 8px;
      position: absolute;
      top: 5px;
      left: 5px;
      z-index: 2;
    }
    #previewArea {
      margin-top: 10px;
      border: 1px dashed #aaa;
      background-color: #fefefe;
      padding: 6px;
      display: none;
      font-size: 12px;
      text-align: left;
    }
    #previewArea ul {
      padding-left: 18px;
      margin: 0;
    }
    h2 {
      font-size: 18px;
      text-align: center;
      margin-bottom: 8px;
    }
  </style>
</head>
<body>
  <h2>くじ引きアプリ</h2>

  <!-- 入力エリア：ここにくじの内容を入力します -->
  <textarea id="inputArea" placeholder="ここにくじの内容を入力..."></textarea>

  <!-- くじの内容を保存するボタン -->
  <div class="corner-buttons">
    <button onclick="saveEntries()">保存</button>
    <button onclick="showPreview()">プレビュー</button>
  </div>

  <!-- プレビューエリア：入力した内容をプレビューします -->
  <div id="previewArea"></div>

  <!-- 戻るボタン：プレビューから戻ります -->
  <button id="backButton" onclick="backToInput()">戻る</button>

  <!-- 抽選ボタンと結果表示エリア -->
  <div id="drawSection">
    <button id="drawButton" onclick="drawLottery()">くじを引く</button>
    <div id="result"></div>
  </div>

  <!-- 抽選履歴の表示エリア -->
  <div id="history"></div>

  <script>
    // ローカルストレージから保存された内容を取得します
    let savedEntries = [];

    // くじ引きの内容を保存します
    function saveEntries() {
      const entries = document.getElementById('inputArea').value.split('\n').map(entry => entry.trim()).filter(entry => entry.length > 0);
      if (entries.length > 0) {
        savedEntries = entries;
        localStorage.setItem('lotteryEntries', JSON.stringify(savedEntries));
        alert('内容が保存されました!');
        document.getElementById('drawSection').style.display = 'block'; // 保存後に抽選ボタンを表示
        showHistory(); // 履歴も表示
      } else {
        alert('内容を入力してください');
      }
    }

    // プレビュー表示
    function showPreview() {
      const previewArea = document.getElementById('previewArea');
      previewArea.innerHTML = '<ul>' + savedEntries.map((entry, index) => `<li>${index + 1}: ${entry}</li>`).join('') + '</ul>';
      previewArea.style.display = 'block';
      document.getElementById('backButton').style.display = 'inline-block';
      document.getElementById('inputArea').style.display = 'none';
      document.querySelector('.corner-buttons').style.display = 'none';
      document.getElementById('drawSection').style.display = 'none'; // プレビュー中は抽選を非表示
    }

    // 入力に戻る
    function backToInput() {
      document.getElementById('previewArea').style.display = 'none';
      document.getElementById('backButton').style.display = 'none';
      document.getElementById('inputArea').style.display = 'block';
      document.querySelector('.corner-buttons').style.display = 'flex';
      document.getElementById('drawSection').style.display = 'block'; // 入力画面に戻ると抽選ボタンを再表示
    }

    // くじ引きの結果を表示します
    function drawLottery() {
      if (savedEntries.length === 0) {
        alert('保存された内容がありません');
        return;
      }
      const randomIndex = Math.floor(Math.random() * savedEntries.length);
      const result = savedEntries[randomIndex];
      document.getElementById('result').innerText = `当選: ${result}`;
      saveHistory(result); // 履歴に保存
    }

    // 抽選履歴を表示
    function showHistory() {
      const historyArea = document.getElementById('history');
      historyArea.innerHTML = '<h3>抽選履歴:</h3>' + savedEntries.map(entry => `<p>${entry}</p>`).join('');
    }

    // 履歴に抽選結果を保存
    function saveHistory(result) {
      let history = localStorage.getItem('lotteryHistory');
      history = history ? JSON.parse(history) : [];
      history.push(result);
      localStorage.setItem('lotteryHistory', JSON.stringify(history));
      showHistory(); // 履歴を更新して表示
    }

    // ロード時に保存されたデータを読み込む
    window.onload = function() {
      const savedData = localStorage.getItem('lotteryEntries');
      if (savedData) {
        savedEntries = JSON.parse(savedData);
        document.getElementById('drawSection').style.display = 'block'; // 保存データがある場合、抽選ボタンを表示
      }
      showHistory(); // 履歴の表示
    }
  </script>
</body>
</html>