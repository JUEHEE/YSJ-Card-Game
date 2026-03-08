<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>나의 이상형 월드컵</title>
    <style>
        body { 
            font-family: 'Pretendard', sans-serif; 
            background-color: #f0f2f5; 
            margin: 0; 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
        }
        .header { text-align: center; margin-top: 20px; }
        .container { 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            height: 70vh; 
            gap: 20px; 
            width: 90%; 
            max-width: 1000px;
        }
        .card { 
            flex: 1; 
            background: white; 
            border-radius: 15px; 
            overflow: hidden; 
            cursor: pointer; 
            transition: transform 0.2s, box-shadow 0.2s; 
            box-shadow: 0 4px 15px rgba(0,0,0,0.1); 
        }
        .card:hover { transform: scale(1.03); box-shadow: 0 8px 25px rgba(0,0,0,0.15); }
        .img-box { width: 100%; height: 400px; }
        .img-box img { width: 100%; height: 100%; object-fit: cover; }
        .name { padding: 15px; text-align: center; font-weight: bold; font-size: 1.2rem; background: #fff; }
        .vs { font-size: 2.5rem; font-weight: 900; color: #ff4757; text-shadow: 2px 2px 4px rgba(0,0,0,0.1); }

        @media (max-width: 768px) {
            .container { flex-direction: column; height: auto; padding-bottom: 30px; }
            .img-box { height: 250px; }
            .vs { margin: 10px 0; }
        }
    </style>
</head>
<body>
    <div id="game-container">
        <div class="header">
            <h1 id="round-title">10강</h1>
            <p id="match-info">1 / 5</p>
        </div>
        
        <div class="container">
            <div class="card" onclick="selectCandidate(0)">
                <div class="img-box">
                    <img id="img-left" src="" alt="왼쪽 후보">
                </div>
                <div class="name" id="name-left">후보 A</div>
            </div>

            <div class="vs">VS</div>

            <div class="card" onclick="selectCandidate(1)">
                <div class="img-box">
                    <img id="img-right" src="" alt="오른쪽 후보">
                </div>
                <div class="name" id="name-right">후보 B</div>
            </div>
        </div>
    </div>

    <div id="result-container" style="display: none; text-align: center; margin-top: 50px;">
        <div class="header">
            <h1>🏆 최종 우승 🏆</h1>
        </div>
        <div id="winner-card-slot"></div>
        <button onclick="location.reload()" style="margin-top:20px; padding:10px 20px; font-size:1.1rem; cursor:pointer; border-radius: 8px; border:none; background: #ff4757; color:white;">다시 하기</button>
    </div>

    <script>
        // 1. 데이터 설정
        let items = [
            { name: "후보 1", src: "yj_0.jpg" },
            { name: "후보 2", src: "yj_1.jpg" },
            { name: "후보 3", src: "yj_2.jpg" },
            { name: "후보 4", src: "yj_3.jpg" },
            { name: "후보 5", src: "yj_4.jpg" },
            { name: "후보 6", src: "yj_5.jpg" },
            { name: "후보 7", src: "yj_6.jpg" },
            { name: "후보 8", src: "yj_7.jpg" },
            { name: "후보 9", src: "yj_8.jpg" },
            { name: "후보 10", src: "yj_9.jpg" }
        ];

        // 초기 섞기
        items.sort(() => Math.random() - 0.5);

        let winners = [];
        let currentIndex = 0;

        function updateDisplay() {
            const roundTitle = document.getElementById("round-title");
            const matchInfo = document.getElementById("match-info");

            // 우승자 결정 시
            if (items.length === 1) {
                showWinner(items[0]);
                return;
            }

            // 라운드 종료 및 다음 라운드 준비
            if (currentIndex >= items.length) {
                items = [...winners];
                winners = [];
                currentIndex = 0;
                
                // 부전승 처리 (인원이 홀수면 마지막 한 명 자동 진출)
                if (items.length > 1 && items.length % 2 !== 0) {
                    const byeCandidate = items.pop();
                    winners.push(byeCandidate);
                    alert(`${byeCandidate.name} 후보가 부전승으로 다음 라운드 진출!`);
                }
            }

            // 라운드 이름 설정
            roundTitle.innerText = items.length === 2 ? "결승" : items.length + "강";
            matchInfo.innerText = `${(currentIndex / 2) + 1} / ${Math.floor(items.length / 2)}`;

            // 이미지 및 이름 삽입
            document.getElementById("img-left").src = items[currentIndex].src;
            document.getElementById("name-left").innerText = items[currentIndex].name;
            document.getElementById("img-right").src = items[currentIndex + 1].src;
            document.getElementById("name-right").innerText = items[currentIndex + 1].name;
        }

        function selectCandidate(index) {
            winners.push(items[currentIndex + index]);
            currentIndex += 2;
            updateDisplay();
        }

        function showWinner(winner) {
            document.getElementById("game-container").style.display = "none";
            const resultContainer = document.getElementById("result-container");
            const slot = document.getElementById("winner-card-slot");
            
            resultContainer.style.display = "block";
            slot.innerHTML = `
                <div class="card" style="width: 350px; margin: 0 auto; cursor: default;">
                    <div class="img-box" style="height: 450px;">
                        <img src="${winner.src}" style="width:100%; height:100%; object-fit:cover;">
                    </div>
                    <div class="name">${winner.name}</div>
                </div>
            `;
        }

        // 게임 시작
        window.onload = updateDisplay;
    </script>
</body>
</html>
