<!DOCTYPE html>
<html>
<head>
    <title>Swimming Fish with Bubbles</title>
    <style>
        body {
            background-color: lightblue;
            overflow: hidden;
            margin: 0;
            font-size:30px; /* تكبير النص */
        }
        #fish {
            position: absolute;
            top: 50%;
            left: 0;
            width: 200px; /* تكبير حجم السمكة */
            height: 100px;
            cursor: pointer;
            animation: swim 5s linear infinite;
        }
        @keyframes swim {
            0% { left: 0; }
            100% { left: 100%; }
        }
        .bubble {
            position: absolute;
            bottom: 0;
            width:50px; /* تكبير حجم الفقاعات */
            height: 45px;
            background-color: rgba(255, 255, 255, 0.6);
            border-radius: 60%;
            animation: rise 5s linear infinite;
            cursor: pointer;
        }
        .bubble:nth-child(odd) {
            animation-duration: 3s;
        }
        .bubble:nth-child(even) {
            animation-duration: 5s;
        }
        @keyframes rise {
            0% { bottom: 0; opacity: 1; }
            100% { bottom: 100%; opacity: 0; }
        }
        #problem {
            position: absolute;
            top: 30%;
            left: 50%;
            transform: translateX(-50%);
            display: none;
            padding: 30px; /* زيادة حجم الحشو */
            background-color: white;
            border: 8px solid black; /* زيادة حجم الحدود */
            font-size: 30px; /* تكبير حجم النص في المشكلة */
        }
        #timer {
            position: absolute;
            top: 10%;
            right: 10%;
            font-size: 50px; /* تكبير حجم المؤقت */
            color: red;
        }                                            #problem input {
            font-size: 30px;
            border-width: 8px; /* تكبير حجم الحدود المحيطة بالجذء */
        }
        .fixed-text {
            font-size: 30px; /* تكبير حجم النص الثابت */
        }
    </style>
</head>
<body>
    <svg id="fish" onclick="stopFish();" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 150 75">
        <!-- جسم السمكة -->
        <ellipse cx="75" cy="37.5" rx="45" ry="22.5" fill="orange" />
        <!-- ذيل السمكة -->
        <polygon points="30,37.5 0,22.5 0,52.5" fill="orange" />
        <!-- عيون السمكة -->
        <circle cx="97.5" cy="30" r="6" fill="white" />
        <circle cx="97.5" cy="30" r="3" fill="black" />
        <!-- فم السمكة -->
        <path d="M112.5,45 Q105,52.5 97.5,45" stroke="black" stroke-width="3" fill="none" />
        <!-- حراشف السمكة -->
        <line x1="52.5" y1="22.5" x2="75" y2="37.5" stroke="white" stroke-width="3" />
        <line x1="52.5" y1="52.5" x2="75" y2="37.5" stroke="white" stroke-width="3" />
        <!-- خياشيم السمكة -->
        <path d="M75,37.5 Q67.5,30 60,37.5 Q67.5,45 75,37.5" stroke="white" stroke-width="3" fill="none" />
        <!-- خطوط الذيل -->
        <line x1="15" y1="22.5" x2="30" y2="37.5" stroke="white" stroke-width="3" />
        <line x1="15" y1="52.5" x2="30" y2="37.5" stroke="white" stroke-width="3" />
    </svg>
    <div id="problem"></div>
    <div id="timer"></div>
    <div class="bubble" style="left: 20%;"></div>
    <div class="bubble" style="left: 40%;"></div>
    <div class="bubble" style="left: 60%;"></div>
    <div class="bubble" style="left: 80%;"></div>
    <div class="fixed-text" style="position: fixed; bottom: 0; left: 0; color: black; font-size: 30px;">Made by Andrew Yaser</div>
    <div class="fixed-text" style="position: fixed; top: 0; left: 0; color: blue; font-size: 30px;">Tap on the fish</div>
    <div class="fixed-text" style="position: fixed; top: 0; right: 0; color: red; font-size: 30px;">Answer before time end</div>
    <div style="position: fixed; top: 0; left: 51%; transform: translateX(-51%); font-size: 30px; color: black;">Made by Andrew Yaser</div>
    <script>
        const problems = [
    { question: '5 + 3 =', answer: 8 },
    { question: '10 - 4 =', answer: 6 },
    { question: '7 + 9 =', answer: 16 },
    { question: '20 - 8 =', answer: 12 },
    { question: '15 + 6 =', answer: 21 },
    { question: '12 - 3 =', answer: 9 },
    { question: '2 + 5 =', answer: 7 },
    { question: '8 - 2 =', answer: 6 },
    { question: '3 + 7 =', answer: 10 },
    { question: '18 - 9 =', answer: 9 },
    { question: '6 + 4 =', answer: 10 },
    { question: '14 - 5 =', answer: 9 },
    { question: '9 + 8 =', answer: 17 },
    { question: '16 - 7 =', answer: 9 },
    { question: '4 + 6 =', answer: 10 },
    { question: '13 - 5 =', answer: 8 },
    { question: '11 + 9 =', answer: 20 },
    { question: '17 - 8 =', answer: 9 },
    { question: '1 + 9 =', answer: 10 },
    { question: '10 - 2 =', answer: 8 },
    { question: '3 + 11 =', answer: 14 },
    { question: '15 - 7 =', answer: 8 },
    { question: '8 + 7 =', answer: 15 },
    { question: '19 - 6 =', answer: 13 },
    { question: '12 + 8 =', answer: 20 },
    { question: '20 - 5 =', answer: 15 },
    { question: '5 + 8 =', answer: 13 },
    { question: '14 - 6 =', answer: 8 },
    { question: '4 + 11 =', answer: 15 },
    { question: '16 - 3 =', answer: 13 },
    { question: '23 + 32 =', answer: 55 },
    { question: '64 - 28 =', answer: 36 },
    { question: '41 + 56 =', answer: 97 },
    { question: '89 - 13 =', answer: 76 },
    { question: '72 + 19 =', answer: 91 },
    { question: '95 - 22 =', answer: 73 },
    { question: '38 + 49 =', answer: 87 },
    { question: '57 - 23 =', answer: 34 },
    { question: '83 + 17 =', answer: 100 },
    { question: '94 - 36 =', answer: 58 },
    { question: '49 + 37 =', answer: 86 },
    { question: '68 - 32 =', answer: 36 },
    { question: '51 + 49 =', answer: 100 },
    { question: '96 - 29 =', answer: 67 },
    { question: '42 + 46 =', answer: 88 },
    { question: '75 - 25 =', answer: 50 },
    { question: '63 + 38 =', answer: 101 },
    { question: '88 - 36 =', answer: 52 },
    { question: '51 + 47 =', answer: 98 },
    { question: '77 - 33 =', answer: 44 },
    { question: '81 + 19 =', answer: 100 },
    { question: '99 - 36 =', answer: 63 },
    { question: '50 + 39 =', answer: 89 },
    { question: '68 - 22 =', answer: 46 },
    { question: '53 + 45 =', answer: 98 },
    { question: '73 - 24 =', answer: 49 },
    { question: '60 + 40 =', answer: 100 },
    { question: '100 - 36 =', answer: 64 },
    { question: '3 + 65 =', answer: 68 },
    { question: '85 - 32 =', answer: 53 },
    { question: '10 + 89 =', answer: 99 },
    { question: '97 - 25 =', answer: 72 },
    { question: '17 + 83 =', answer: 100 },
    { question: '92 - 36 =', answer: 56 },
    { question: '41 + 59 =', answer: 100 },
    { question: '70 - 25 =', answer: 45 },
    { question: '58 + 42 =', answer: 100 },
    { question: '79 - 36 =', answer: 43 },
    { question: '10 + 90 =', answer: 100 },
    { question: '99 - 36 =', answer: 63 },
    { question: '23 + 76 =', answer: 99 },
    { question: '91 - 36 =', answer: 55 },
    { question: '35 + 65 =', answer: 100 },
    { question: '97 - 36 =', answer: 61 },
    { question: '11 + 89 =', answer: 100 },
    { question: '99 - 35 =', answer: 64 },
    { question: '31 + 69 =', answer: 100 },
    { question: '97 - 36 =', answer: 61 },
    { question: '14 + 86 =', answer: 100 },
    { question: '100 - 37 =', answer: 63 },
    { question: '23 + 77 =', answer: 100 },
    { question: '99 - 35 =', answer: 64 },
    { question: '34 + 66 =', answer: 100 },
    { question: '98 - 36 =', answer: 62 },
    { question: '18 + 82 =', answer: 100 },
    { question: '100 - 35 =', answer: 65 }
];


        let timerInterval;
        let timeLeft = 30;
        function startTimer() {
            const timer = document.getElementById('timer');
            timer.innerText = timeLeft;
            timerInterval = setInterval(() => {
                timeLeft--;
                if (timeLeft >= 0) {
                    timer.innerText = timeLeft;
                } else {
                    clearInterval(timerInterval);
                    timer.innerText = '';
                    const problem = document.getElementById('problem');
                    problem.innerHTML = '<span style="color: red;">Time end!</span>';
                    setTimeout(() => {
                        problem.innerHTML = '';
                        problem.style.display = 'none'; // hide problem box
                        startFish();
                    }, 2000);
                }
            }, 1000);
        }
        function stopTimer() {
            clearInterval(timerInterval);
            timeLeft = 30;
            document.getElementById('timer').innerText = '';
        }
        function startFish() {
            const fish = document.getElementById('fish');
            fish.style.animationPlayState = 'running';
        }
        function stopFish() {
            stopTimer();
            const fish = document.getElementById('fish');
            const problem = document.getElementById('problem');
            fish.style.animationPlayState = 'paused';
            problem.style.display = 'block';
            startTimer();
            const randomProblem = problems[Math.floor(Math.random() * problems.length)];
            problem.innerHTML = `${randomProblem.question} <input type="text" id="answer" style="font-size: 26px;"> <button onclick="checkAnswer(${randomProblem.answer});" style="font-size: 26px;">Submit</button>`;
        }
        function checkAnswer(correctAnswer) {
            stopTimer();
            const answer = document.getElementById('answer').value;
            const problem = document.getElementById('problem');
            if (parseFloat(answer) == correctAnswer) {
                problem.innerHTML = '<span style="color: green;">Right!</span>';
            } else {
                problem.innerHTML = '<span style="color: red;">wrong!</span>';
            }
            setTimeout(() => {
                const fish = document.getElementById('fish');
                fish.style.animationPlayState = 'running';
                problem.style.display = 'none';
            }, 2000);
        }
        document.querySelectorAll('.bubble').forEach(bubble => {
            bubble.addEventListener('click', () => {
                bubble.innerText = 'made by andrew';
            });
        });
    </script>
</body>
</html>
