<!DOCTYPE html>
<html>
<head>
    <title>Swimming Fish with Bubbles</title>
    <style>
        body {
            background-color: lightblue;
            overflow: hidden;
            margin: 0;
            font-size: 24px; /* تكبير النص */
        }
        #fish {
            position: absolute;
            top: 50%;
            left: 0;
            width: 150px; /* تكبير حجم السمكة */
            height: 75px;
            cursor: pointer;
            animation: swim 4s linear infinite;
        }
        @keyframes swim {
            0% { left: 0; }
            100% { left: 100%; }
        }
        .bubble {
            position: absolute;
            bottom: 0;
            width: 30px; /* تكبير حجم الفقاعات */
            height: 30px;
            background-color: rgba(255, 255, 255, 0.6);
            border-radius: 50%;
            animation: rise 3s linear infinite;
            cursor: pointer;
        }
        .bubble:nth-child(odd) {
            animation-duration: 2s;
        }
        .bubble:nth-child(even) {
            animation-duration: 4s;
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
            padding: 20px; /* زيادة حجم الحشو */
            background-color: white;
            border: 2px solid black; /* زيادة حجم الحدود */
            font-size: 24px; /* تكبير حجم النص في المشكلة */
        }
        #timer {
            position: absolute;
            top: 10%;
            right: 10%;
            font-size: 36px; /* تكبير حجم المؤقت */
            color: red;
        }
        .fixed-text {
            font-size: 24px; /* تكبير حجم النص الثابت */
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
    <div class="fixed-text" style="position: fixed; bottom: 0; left: 0; color: black;">Made by Andrew Yaser</div>
    <div class="fixed-text" style="position: fixed; top: 0; left: 0; color: blue;">Tap on the fish</div>
    <div class="fixed-text" style="position: fixed; top: 0; right: 0; color: red;">Answer before time end</div>                              <div style="position: fixed; top: 0; left: 50%; transform: translateX(-50%); font-size: 8px; color: black;">Made by Andrew Yaser</div>

    <script>
        const problems = [
            { question: '5 + 5 =', answer: 10 },
            { question: '6 × 7 =', answer: 42 },
            { question: '100 - 86 =', answer: 14 },
            { question: '200 ÷ 10 =', answer: 20 },
            { question: '10 + 10 =', answer: 20 },
            { question: '7 × 8 =', answer: 56 },
            { question: '15 - 3 =', answer: 12 },
            { question: '24 ÷ 4 =', answer: 6 },
            { question: '1 + 2 =', answer: 3 },
            { question: '4 × 5 =', answer: 20 },
            { question: '5 - 3 =', answer: 2 },
            { question: '10 ÷ 2 =', answer: 5 },
            { question: '3 + 3 =', answer: 6 },
            { question: '8 × 9 =', answer: 72 },
            { question: '12 - 7 =', answer: 5 },
            { question: '30 ÷ 6 =', answer: 5 },
            { question: '2 + 8 =', answer: 10 },
            { question: '6 × 6 =', answer: 36 },
            { question: '20 - 12 =', answer: 8 },
            { question: '40 ÷ 5 =', answer: 8 },
            { question: '4 + 6 =', answer: 10 },
            { question: '9 × 3 =', answer: 27 },
            { question: '18 - 5 =', answer: 13 },
            { question: '50 ÷ 10 =', answer: 5 },
            { question: '7 + 3 =', answer: 10 }
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
            problem.innerHTML = `${randomProblem.question} <input type="text" id="answer" style="font-size: 24px;"> <button onclick="checkAnswer(${randomProblem.answer});" style="font-size: 24px;">Submit</button>`;
        }
        function checkAnswer(correctAnswer) {
            stopTimer();
            const answer = document.getElementById('answer').value;
            const problem = document.getElementById('problem');
            if (parseFloat(answer) == correctAnswer) {
                problem.innerHTML = '<span style="color: green;">Bravo!</span>';
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
                bubble.innerText = 'Andro';
            });
        });
    </script>
</body>
</html>
