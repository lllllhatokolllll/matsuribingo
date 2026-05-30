<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>とうものビンゴ</title>
    <style>
        :root { --bg-color: #071126; --accent-color: #ffa502; --primary-color: #ff4757; }
        * { box-sizing: border-box; }
        body { font-family: sans-serif; text-align: center; background-color: var(--bg-color); margin: 0; padding: 10px; display: flex; flex-direction: column; align-items: center; min-height: 100vh; }
        
        /* 案内テキスト */
        .instruction { width: 90%; max-width: 420px; font-size: 14px; font-weight: bold; background-color: rgba(255, 165, 2, 0.95); color: #071126; padding: 12px; border-radius: 15px; border: 2px solid #ffffff; box-shadow: 0 4px 10px rgba(0,0,0,0.3); margin-bottom: 15px; z-index: 10; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.02); } 100% { transform: scale(1); } }

        /* ビンゴ画面 */
        .bingo-outer { width: 100%; max-width: 420px; overflow: hidden; border-radius: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.5); position: relative; }
        .bingo-container { position: relative; width: 142%; left: -21%; }
        .bg-image { width: 100%; height: auto; display: block; }
        .bingo-board { position: absolute; top: 25.4%; left: 29.2%; width: 41.6%; height: 70.8%; display: grid; grid-template-columns: repeat(3, 1fr); grid-template-rows: repeat(3, 1fr); gap: 4.5% 3.5%; }
        .cell { background-color: rgba(255, 255, 255, 0.98); border: 1.5px solid #bdc3c7; border-radius: 6px; display: flex; align-items: center; justify-content: center; padding: 5px; cursor: pointer; position: relative; overflow: hidden; aspect-ratio: 1 / 1; }
        .cell-text { font-size: 12.5px; font-weight: bold; line-height: 1.3; color: #1a252f; word-break: break-all; z-index: 6; }
        .stamp-video { display: none; position: absolute; width: 100%; height: 100%; top: 0; left: 0; object-fit: cover; z-index: 7; pointer-events: none; }
        .cell.selected .cell-text { opacity: 0.15; }
        .cell.selected .stamp-video { display: block; }

        /* 景品引き換え画面（最初は見えない） */
        .result-screen { display: none; width: 90%; max-width: 420px; margin-top: 20px; padding: 20px; background: white; border-radius: 15px; text-align: center; }
    </style>
</head>
<body>

    <div class="instruction" id="instruction">
        上のマスをタップして「うちわスタンプ」を集めよう！<br>1列そったら、画面をスタッフにみせてね♬
    </div>

    <div class="bingo-outer" id="bingoWrap">
        <div class="bingo-container">
            <img src="haikei.png" alt="背景" class="bg-image">
            <div class="bingo-board" id="board"></div>
        </div>
    </div>

    <div class="result-screen" id="resultScreen">
        <h2 style="color: #ff4757;">🎆 ビンゴ達成！ 🎆</h2>
        <p>スタッフにこの画面を見せてね！</p>
    </div>

    <script>
        const items = [
            '<ruby>舞台<rt>ぶたい</rt></ruby>に<ruby>上<rt>あ</rt></ruby>がって<br>たのしんだ',
            '<ruby>絶対<rt>ぜったい</rt></ruby>に<br><ruby>来年<rt>らいねん</rt></ruby>も来る！！',
            '加藤ふみあきさんを<br>みつけた！',
            'オタゲーを<br>観た！',
            'とうもの会の<br>ゲームに<ruby>参加<rt>さんか</rt></ruby>した', 
            '<ruby>屋台<rt>やたい</rt></ruby>でお買物をした',
            '<ruby>盆踊<rt>ぼんおど</rt></ruby>りを<br>1<ruby>曲以上<rt>きょくいじょう</rt></ruby>おどった',
            '学校の<br><ruby>先生<rt>せんせい</rt></ruby>に会った',
            '<ruby>消防団<rt>しょうぼうだん</rt></ruby>の人に<br><ruby>挨拶<rt>あいさつ</rt></ruby>した'
        ];

        const board = document.getElementById('board');
        const cells = [];

        items.forEach((text, index) => {
            const cell = document.createElement('div');
            cell.classList.add('cell');
            cell.innerHTML = `<div class="cell-text">${text}</div><video class="stamp-video" src="uchiwa.mp4" loop muted playsinline></video>`;
            
            cell.addEventListener('click', () => {
                cell.classList.toggle('selected');
                const video = cell.querySelector('video');
                if(cell.classList.contains('selected')) video.play();
                else { video.pause(); video.currentTime = 0; }
                checkBingo();
            });
            board.appendChild(cell);
            cells.push(cell);
        });

        // 揃ったかチェックする関数
        function checkBingo() {
            const lines = [[0,1,2],[3,4,5],[6,7,8],[0,3,6],[1,4,7],[2,5,8],[0,4,8],[2,4,6]];
            for(let line of lines) {
                if(cells[line[0]].classList.contains('selected') && 
                   cells[line[1]].classList.contains('selected') && 
                   cells[line[2]].classList.contains('selected')) {
                    
                    document.getElementById('bingoWrap').style.display = 'none';
                    document.getElementById('instruction').style.display = 'none';
                    document.getElementById('resultScreen').style.display = 'block';
                }
            }
        }
    </script>
</body>
</html>
