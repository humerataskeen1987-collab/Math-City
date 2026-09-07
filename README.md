<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Scan the Math City</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: linear-gradient(#8ed8ff, #d9f7ff);
    text-align: center;
    overflow-x: hidden;
}

header {
    background: #25245c;
    color: white;
    padding: 18px;
    box-shadow: 0 5px 15px #555;
}

h1 {
    margin: 5px;
    font-size: 36px;
}

.subtitle {
    font-size: 18px;
}

#score {
    font-size: 20px;
    margin-top: 8px;
}

.city {
    min-height: 650px;
    padding: 30px 20px 80px;
    position: relative;
    background:
        linear-gradient(#9ee7ff 0 70%, #73c45c 70% 100%);
}

.sun {
    position: absolute;
    right: 50px;
    top: 30px;
    width: 80px;
    height: 80px;
    background: #ffd93d;
    border-radius: 50%;
    box-shadow: 0 0 35px #ffd93d;
    animation: sunPulse 2s infinite alternate;
}

@keyframes sunPulse {
    from { transform: scale(1); }
    to { transform: scale(1.12); }
}

.cloud {
    position: absolute;
    background: white;
    border-radius: 50px;
    width: 130px;
    height: 45px;
    opacity: .85;
    animation: cloudMove 18s linear infinite;
}

.cloud:before,
.cloud:after {
    content: "";
    position: absolute;
    background: white;
    border-radius: 50%;
}

.cloud:before {
    width: 55px;
    height: 55px;
    left: 20px;
    top: -25px;
}

.cloud:after {
    width: 70px;
    height: 70px;
    right: 15px;
    top: -35px;
}

.cloud1 { top: 100px; left: -150px; }
.cloud2 { top: 180px; left: -300px; animation-delay: 7s; }

@keyframes cloudMove {
    from { left: -180px; }
    to { left: 110%; }
}

.buildings {
    position: relative;
    z-index: 2;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 25px;
    margin-top: 70px;
}

.building {
    width: 180px;
    min-height: 240px;
    border: 5px solid #222;
    border-radius: 12px 12px 5px 5px;
    cursor: pointer;
    color: white;
    padding: 15px 10px;
    position: relative;
    transition: .3s;
    animation: buildingBounce 3s ease-in-out infinite;
}

.building:hover {
    transform: translateY(-15px) scale(1.05);
}

.building:nth-child(2) { animation-delay: .3s; }
.building:nth-child(3) { animation-delay: .6s; }
.building:nth-child(4) { animation-delay: .9s; }
.building:nth-child(5) { animation-delay: 1.2s; }

@keyframes buildingBounce {
    0%,100% { transform: translateY(0); }
    50% { transform: translateY(-7px); }
}

.integer { background: #e63946; height: 280px; }
.fraction { background: #f77f00; height: 250px; }
.algebra { background: #8338ec; height: 300px; }
.data { background: #0077b6; height: 260px; }
.shapes { background: #2a9d8f; height: 270px; }

.roof {
    position: absolute;
    top: -45px;
    left: -8px;
    width: 180px;
    height: 50px;
    background: #333;
    clip-path: polygon(50% 0,100% 100%,0 100%);
}

.windows {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
    margin-top: 25px;
}

.window {
    height: 45px;
    background: #fff59d;
    border: 3px solid #222;
    animation: light 1.5s infinite alternate;
}

@keyframes light {
    from { opacity: .55; }
    to { opacity: 1; }
}

.door {
    width: 50px;
    height: 65px;
    background: #4b2e1e;
    margin: 15px auto 0;
    border: 3px solid #111;
}

.robot {
    position: fixed;
    bottom: 20px;
    left: 25px;
    z-index: 10;
    font-size: 65px;
    animation: robotMove 4s ease-in-out infinite;
}

@keyframes robotMove {
    0%,100% { transform: translateX(0) rotate(-3deg); }
    50% { transform: translateX(40px) rotate(3deg); }
}

button {
    background: #25245c;
    color: white;
    border: none;
    border-radius: 12px;
    padding: 13px 25px;
    font-size: 17px;
    cursor: pointer;
    margin: 8px;
}

button:hover {
    transform: scale(1.05);
    background: #403f91;
}

.modal {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,.7);
    z-index: 20;
    align-items: center;
    justify-content: center;
}

.modal-box {
    background: white;
    width: 90%;
    max-width: 600px;
    max-height: 90vh;
    overflow-y: auto;
    border-radius: 20px;
    padding: 25px;
    animation: pop .3s ease;
}

@keyframes pop {
    from { transform: scale(.5); opacity: 0; }
    to { transform: scale(1); opacity: 1; }
}

.question {
    background: #f1f1f1;
    padding: 15px;
    margin: 15px 0;
    border-radius: 12px;
}

.option {
    display: block;
    width: 100%;
    margin: 7px 0;
    background: #e8e8e8;
    color: #222;
}

.correct {
    background: #7bed9f !important;
}

.wrong {
    background: #ff7675 !important;
}

.progress {
    height: 15px;
    background: #ddd;
    border-radius: 20px;
    overflow: hidden;
}

.progress-bar {
    height: 100%;
    width: 0%;
    background: #2a9d8f;
    transition: .5s;
}

@media(max-width:600px) {
    h1 { font-size: 27px; }
    .building { width: 155px; }
    .roof { width: 155px; }
}
</style>
</head>

<body>

<header>
    <h1>🏙️ SCAN THE MATH CITY 🧮</h1>
    <div class="subtitle">Explore • Solve • Learn • Have Fun!</div>
    <div id="score">⭐ Score: 0</div>
</header>

<div class="city">

    <div class="sun"></div>
    <div class="cloud cloud1"></div>
    <div class="cloud cloud2"></div>

    <button onclick="startTour()">🚀 START TOUR</button>

    <div class="buildings">

        <div class="building integer" onclick="openBuilding('integer')">
            <div class="roof"></div>
            <h2>🔢 Integer Tower</h2>
            <p>Explore positive & negative numbers</p>
            <div class="windows">
                <div class="window"></div>
                <div class="window"></div>
                <div class="window"></div>
                <div class="window"></div>
            </div>
            <div class="door"></div>
        </div>

        <div class="building fraction" onclick="openBuilding('fraction')">
            <div class="roof"></div>
            <h2>🍕 Fraction Café</h2>
            <p>Learn fractions with tasty maths!</p>
            <div class="windows">
                <div class="window"></div>
                <div class="window"></div>
                <div class="window"></div>
                <div class="window"></div>
            </div>
            <div class="door"></div>
        </div>

        <div class="building algebra" onclick="openBuilding('algebra')">
            <div class="roof"></div>
            <h2>🧪 Algebra Lab</h2>
            <p>Discover the mystery of x</p>
            <div class="windows">
                <div class="window"></div>
                <div class="window"></div>
                <div class="window"></div>
                <div class="window"></div>
            </div>
            <div class="door"></div>
        </div>

        <div class="building data" onclick="openBuilding('data')">
            <div class="roof"></div>
            <h2>📊 Data Centre</h2>
            <p>Explore graphs and data</p>
            <div class="windows">
                <div class="window"></div>
                <div class="window"></div>
                <div class="window"></div>
                <div class="window"></div>
            </div>
            <div class="door"></div>
        </div>

        <div class="building shapes" onclick="openBuilding('shapes')">
            <div class="roof"></div>
            <h2>🏟️ Shapes Stadium</h2>
            <p>Discover amazing shapes</p>
            <div class="windows">
                <div class="window"></div>
                <div class="window"></div>
                <div class="window"></div>
                <div class="window"></div>
            </div>
            <div class="door"></div>
        </div>

    </div>
</div>

<div class="robot">🤖</div>

<div class="modal" id="modal">
    <div class="modal-box">
        <h2 id="modalTitle"></h2>

        <div class="progress">
            <div class="progress-bar" id="progressBar"></div>
        </div>

        <div id="questions"></div>

        <button onclick="closeModal()">🏠 Back to City</button>
    </div>
</div>

<script>

let score = 0;

const questions = {

integer: {
    title: "🔢 Integer Tower",
    qs: [
        {
            q: "Which number is greater?",
            options: ["−5", "−2", "−8", "−10"],
            answer: "−2"
        },
        {
            q: "What is 7 + (−3)?",
            options: ["3", "4", "5", "10"],
            answer: "4"
        },
        {
            q: "What is 5 − 8?",
            options: ["3", "−3", "13", "−13"],
            answer: "−3"
        }
    ]
},

fraction: {
    title: "🍕 Fraction Café",
    qs: [
        {
            q: "What is ½ + ¼?",
            options: ["¾", "⅓", "⅔", "1"],
            answer: "¾"
        },
        {
            q: "What is ¾ − ¼?",
            options: ["¼", "½", "⅔", "1"],
            answer: "½"
        },
        {
            q: "Which fraction equals ½?",
            options: ["2/4", "3/4", "1/3", "2/3"],
            answer: "2/4"
        }
    ]
},

algebra: {
    title: "🧪 Algebra Lab",
    qs: [
        {
            q: "If x + 2 = 5, what is x?",
            options: ["2", "3", "4", "7"],
            answer: "3"
        },
        {
            q: "If x − 4 = 6, what is x?",
            options: ["2", "10", "12", "−2"],
            answer: "10"
        },
        {
            q: "If 2x = 10, what is x?",
            options: ["2", "4", "5", "8"],
            answer: "5"
        }
    ]
},

data: {
    title: "📊 Data Centre",
    qs: [
        {
            q: "Monday has 4 books and Tuesday has 7 books. Which day has more?",
            options: ["Monday", "Tuesday", "Both", "Neither"],
            answer: "Tuesday"
        },
        {
            q: "A class has 12 boys and 8 girls. How many students altogether?",
            options: ["18", "20", "22", "24"],
            answer: "20"
        },
        {
            q: "Scores are 5, 8, 6, 8. What is the highest score?",
            options: ["5", "6", "7", "8"],
            answer: "8"
        }
    ]
},

shapes: {
    title: "🏟️ Shapes Stadium",
    qs: [
        {
            q: "How many sides does a triangle have?",
            options: ["2", "3", "4", "5"],
            answer: "3"
        },
        {
            q: "How many sides does a square have?",
            options: ["3", "4", "5", "6"],
            answer: "4"
        },
        {
            q: "Which shape has no corners?",
            options: ["Triangle", "Square", "Circle", "Rectangle"],
            answer: "Circle"
        }
    ]
}

};

function openBuilding(type) {

    const data = questions[type];

    document.getElementById("modalTitle").textContent = data.title;

    const container = document.getElementById("questions");
    container.innerHTML = "";

    data.qs.forEach((item, index) => {

        const box = document.createElement("div");
        box.className = "question";

        const title = document.createElement("h3");
        title.textContent = (index + 1) + ". " + item.q;

        box.appendChild(title);

        item.options.forEach(option => {

            const btn = document.createElement("button");
            btn.className = "option";
            btn.textContent = option;

            btn.onclick = function() {

                const buttons = box.querySelectorAll(".option");

                buttons.forEach(b => b.disabled = true);

                if(option === item.answer) {
                    btn.classList.add("correct");
                    score += 10;
                    document.getElementById("score").textContent =
                        "⭐ Score: " + score;
                } else {
                    btn.classList.add("wrong");

                    buttons.forEach(b => {
                        if(b.textContent === item.answer) {
                            b.classList.add("correct");
                        }
                    });
                }

                updateProgress();
            };

            box.appendChild(btn);
        });

        container.appendChild(box);
    });

    document.getElementById("modal").style.display = "flex";
    updateProgress();
}

function updateProgress() {

    const answered =
        document.querySelectorAll(".option:disabled").length;

    const total = 15;

    const percentage =
        Math.min(100, (answered / total) * 100);

    document.getElementById("progressBar").style.width =
        percentage + "%";
}

function closeModal() {
    document.getElementById("modal").style.display = "none";
}

function startTour() {

    alert(
        "🚀 Welcome to Math City!\n\n" +
        "Visit all 5 buildings and solve 3 questions in each building.\n\n" +
        "Good luck, Math Explorer! 🧮"
    );
}

window.onclick = function(event) {

    if(event.target === document.getElementById("modal")) {
        closeModal();
    }

};

</script>

</body>
</html>
