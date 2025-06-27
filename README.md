<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>あみだくじ</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      padding: 20px;
      background: #f9f9f9;
    }
    input {
      width: 80%;
      max-width: 250px;
      padding: 8px;
      margin: 5px;
      font-size: 1em;
    }
    button {
      padding: 10px 20px;
      font-size: 1em;
      margin: 10px;
      background-color: #3498db;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }
    canvas {
      border: 1px solid #ccc;
      max-width: 100%;
      height: auto;
    }
    #results {
      margin-top: 20px;
      font-size: 1.1em;
      background: #fff;
      padding: 10px;
      border-radius: 8px;
      box-shadow: 0 0 5px rgba(0,0,0,0.1);
    }
  </style>
</head>
<body>
  <h1>あみだくじ</h1>
  <p>名前と景品を入力してスタート！</p>

  <div>
    <h3>参加者の名前（4人）</h3>
    <input id="name1" placeholder="例: 太郎"><br>
    <input id="name2" placeholder="例: 花子"><br>
    <input id="name3" placeholder="例: 次郎"><br>
    <input id="name4" placeholder="例: みどり"><br>

    <h3>景品（4つ）</h3>
    <input id="item1" placeholder="例: お菓子"><br>
    <input id="item2" placeholder="例: ドリンク"><br>
    <input id="item3" placeholder="例: 映画チケット"><br>
    <input id="item4" placeholder="例: 何もなし"><br>

    <button onclick="startAmida()">あみだくじ開始！</button>
  </div>

  <canvas id="canvas" width="400" height="400"></canvas>
  <div id="results"></div>

  <script>
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    const width = canvas.width;
    const height = canvas.height;
    const margin = width / 5;
    let lines = [];

    function shuffle(array) {
      return array.sort(() => Math.random() - 0.5);
    }

    function startAmida() {
      ctx.clearRect(0, 0, width, height);
      lines = [];

      const names = [
        document.getElementById('name1').value || "A",
        document.getElementById('name2').value || "B",
        document.getElementById('name3').value || "C",
        document.getElementById('name4').value || "D"
      ];

      let items = [
        document.getElementById('item1').value || "景品1",
        document.getElementById('item2').value || "景品2",
        document.getElementById('item3').value || "景品3",
        document.getElementById('item4').value || "景品4"
      ];

      items = shuffle(items);

      // 縦線
      for (let i = 1; i <= 4; i++) {
        ctx.beginPath();
        ctx.moveTo(margin * i, 0);
        ctx.lineTo(margin * i, height);
        ctx.stroke();
      }

      // 横線（ランダムに5本くらい）
      for (let y = 40; y < height - 40; y += 40) {
        const x = Math.floor(Math.random() * 3) + 1;
        lines.push({ x, y });
        ctx.beginPath();
        ctx.moveTo(margin * x, y);
        ctx.lineTo(margin * (x + 1), y);
        ctx.stroke();
      }

      // あみだロジック
      const results = [];

      for (let i = 0; i < 4; i++) {
        let pos = i + 1;
        let x = margin * pos;
        let y = 0;

        lines.forEach(line => {
          if (Math.abs(line.y - y) < 20) {
            if (line.x === pos) pos++;
            else if (line.x === pos - 1) pos--;
          }
          y = line.y;
        });

        results.push(`${names[i]} → ${items[pos - 1]}`);
      }

      // 結果表示
      document.getElementById('results').innerHTML = "<h3>🎉 結果発表 🎉</h3>" + results.join("<br>");
    }
  </script>
</body>
</html>
✅ このファイルの使い方
上記コードをコピーして index.html という名前で保存

GitHub リポジトリにアップロード

GitHub Pages を設定（手順はこちら）

公開URLでスマホ・PCから誰でも使えます！

💬 ご希望があればさらに追加できます
景品や結果をランダムに「画像」付きにしたい

あみだの経路をアニメーションで表示

QRコード生成してその場で友達と共有

なども可能です。必要ならお気軽にリクエストしてください！











