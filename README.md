# YSJ-Card-Game
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>나의 이상형 월드컵</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="header">
        <h1 id="round-title">16강</h1>
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
body { font-family: 'Pretendard', sans-serif; background-color: #f0f2f5; margin: 0; display: flex; flex-direction: column; align-items: center; }
.header { text-align: center; margin-top: 20px; }
.container { display: flex; align-items: center; justify-content: center; height: 80vh; gap: 20px; width: 90%; }
.card { flex: 1; background: white; border-radius: 15px; overflow: hidden; cursor: pointer; transition: transform 0.2s; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
.card:hover { transform: scale(1.03); }
.img-box { width: 100%; height: 400px; overflow: hidden; }
.img-box img { width: 100%; height: 100%; object-fit: cover; }
.name { padding: 15px; text-align: center; font-weight: bold; font-size: 1.2rem; }
.vs { font-size: 2rem; font-weight: 900; color: #ff4757; }

@media (max-width: 768px) {
    .container { flex-direction: column; }
    .img-box { height: 250px; }
}
// 1. 사진 데이터 설정 (업로드한 10장)
let items = [
    { name: "후보 0", src: "yj_0.jpg" },
    { name: "후보 1", src: "yj_1.jpg" },
    { name: "후보 2", src: "yj_2.jpg" },
    { name: "후보 3", src: "yj_3.jpg" },
    { name: "후보 4", src: "yj_4.jpg" },
    { name: "후보 5", src: "yj_5.jpg" },
    { name: "후보 6", src: "yj_6.jpg" },
    { name: "후보 7", src: "yj_7.jpg" },
    { name: "후보 8", src: "yj_8.jpg" },
    { name: "후보 9", src: "yj_9.jpg" }
];

// 섞기 (랜덤 대진)
items.sort(() => Math.random() - 0.5);

let winners = [];
let currentIndex = 0;

function updateDisplay() {
    // 승자가 한 명 남았을 때 (우승)
    if (items.length === 1) {
        document.body.innerHTML = `
            <div class="header">
                <h1>🏆 최종 우승 🏆</h1>
                <div class="card" style="width: 300px; margin: 20px auto;">
                    <img src="${items[0].src}" style="width:100%;">
                    <div class="name">${items[0].name}</div>
                </div>
                <button onclick="location.reload()" style="padding:10px 20px; cursor:pointer;">다시 하기</button>
            </div>`;
        return;
    }

    // 라운드 종료 및 다음 라운드 준비
    if (currentIndex >= items.length) {
        items = [...winners];
        winners = [];
        currentIndex = 0;
        // 10강 다음은 5강이 되므로 적절히 제목 수정
        document.getElementById("round-title").innerText = items.length === 2 ? "결승" : items.length + "강";
    }

    // 부전승 처리 (홀수일 때)
    if (currentIndex === items.length - 1) {
        winners.push(items[currentIndex]);
        items = [...winners];
        winners = [];
        currentIndex = 0;
    }

    document.getElementById("match-info").innerText = `${(currentIndex / 2) + 1} / ${Math.floor(items.length / 2)}`;
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

// 첫 화면 실행
document.getElementById("round-title").innerText = "10강";
updateDisplay();
    <script src="script.js"></script>
</body>
</html>
