# PMO-EPA-Revision
Interactive Game
<div id="pmo-game">
  <style>
    #pmo-game {
      font-family: Arial, Helvetica, sans-serif;
      background: #f2f5f7;
      padding: 20px;
      color: #1f2933;
    }

    #pmo-game .phone {
      max-width: 760px;
      margin: auto;
      background: #000;
      border-radius: 36px;
      padding: 16px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.25);
    }

    #pmo-game .screen {
      background: #fff;
      border-radius: 26px;
      padding: clamp(18px, 4vw, 32px);
      min-height: 620px;
    }

    #pmo-game h1 {
      font-size: clamp(1.7rem, 4vw, 2.5rem);
      color: #003b5c;
      margin: 0 0 10px;
      text-align: center;
    }

    #pmo-game h2 {
      font-size: clamp(1.2rem, 3vw, 1.6rem);
      color: #003b5c;
    }

    #pmo-game p, #pmo-game button {
      font-size: clamp(1rem, 2.5vw, 1.15rem);
      line-height: 1.5;
    }

    #pmo-game .card {
      background: #eef6fa;
      border-left: 6px solid #003b5c;
      border-radius: 16px;
      padding: 18px;
      margin: 18px 0;
    }

    #pmo-game .progress {
      background: #d8e3ea;
      border-radius: 20px;
      overflow: hidden;
      height: 18px;
      margin: 16px 0;
    }

    #pmo-game .bar {
      background: #2e7d32;
      height: 100%;
      width: 0%;
      transition: width 0.3s ease;
    }

    #pmo-game button {
      width: 100%;
      border: none;
      border-radius: 14px;
      padding: 14px;
      margin: 8px 0;
      cursor: pointer;
      font-weight: bold;
    }

    #pmo-game .start, #pmo-game .next, #pmo-game .restart {
      background: #2e7d32;
      color: white;
    }

    #pmo-game .answer {
      background: #003b5c;
      color: white;
      text-align: left;
    }

    #pmo-game .answer:hover {
      background: #00557f;
    }

    #pmo-game .correct {
      background: #2e7d32 !important;
    }

    #pmo-game .wrong {
      background: #b3261e !important;
    }

    #pmo-game .feedback {
      border-radius: 14px;
      padding: 14px;
      margin-top: 14px;
      display: none;
      font-weight: bold;
    }

    #pmo-game .good {
      background: #e4f4e6;
      color: #1b5e20;
    }

    #pmo-game .bad {
      background: #fdecea;
      color: #8a1c14;
    }

    #pmo-game .score {
      text-align: center;
      font-size: clamp(1.4rem, 4vw, 2rem);
      font-weight: bold;
      color: #003b5c;
    }

    #pmo-game .small {
      font-size: 0.95rem;
      color: #52606d;
      text-align: center;
    }
  </style>

  <div class="phone">
    <div class="screen">
      <h1>PMO EPA Revision Challenge</h1>
      <p class="small">Property Maintenance Operative Knowledge Check</p>

      <div id="startScreen">
        <div class="card">
          <h2>Game Instructions</h2>
          <p>
            Answer each scenario-based question. You will receive instant feedback
            to help prepare for the PMO EPA online knowledge test and professional discussion.
          </p>
          <p><strong>Target:</strong> Aim for 85% or above.</p>
        </div>
        <button class="start" onclick="startGame()">Start Challenge</button>
      </div>

      <div id="quizScreen" style="display:none;">
        <p id="questionNumber"></p>
        <div class="progress"><div class="bar" id="progressBar"></div></div>
        <div class="card">
          <h2 id="questionText"></h2>
          <div id="answers"></div>
          <div id="feedback" class="feedback"></div>
        </div>
        <button id="nextBtn" class="next" onclick="nextQuestion()" style="display:none;">Next Question</button>
      </div>

      <div id="endScreen" style="display:none;">
        <div class="card">
          <h2>Challenge Complete</h2>
          <p class="score" id="finalScore"></p>
          <p id="finalMessage"></p>
        </div>
        <button class="restart" onclick="restartGame()">Try Again</button>
      </div>
    </div>
  </div>

  <script>
    const questions = [
      {
        q: "You arrive to complete a repair in a classroom. What should be your first action before starting practical work?",
        a: [
          "Start the repair immediately to reduce disruption",
          "Check the work area, risks, PPE and access requirements",
          "Ask the customer to leave and begin the task",
          "Collect tools first and assess the area afterwards"
        ],
        correct: 1,
        feedback: "Correct. A safe and tidy work area must be established before work begins."
      },
      {
        q: "A socket needs removing before a wall repair. What is the safest action?",
        a: [
          "Tape over the socket and continue working",
          "Ask another learner to hold the socket aside",
          "Follow safe isolation procedures before work starts",
          "Remove the socket carefully using insulated gloves only"
        ],
        correct: 2,
        feedback: "Correct. Electrical work must be safely isolated before starting."
      },
      {
        q: "You discover damage that is outside your level of competence. What should you do?",
        a: [
          "Attempt the repair carefully to gain experience",
          "Ignore the issue if it is not part of the task",
          "Report and escalate the issue to the correct person",
          "Complete a temporary repair without telling anyone"
        ],
        correct: 2,
        feedback: "Correct. PMOs must recognise limits of competence and escalate safely."
      },
      {
        q: "Why should manufacturer instructions be followed when fitting materials or components?",
        a: [
          "They help ensure the work is safe, suitable and compliant",
          "They are only needed when using unfamiliar materials",
          "They replace the need for risk assessments and checks",
          "They are mainly used to reduce paperwork after the job"
        ],
        correct: 0,
        feedback: "Correct. Manufacturer guidance supports safe, compliant and durable work."
      },
      {
        q: "A customer reports a leaking waste pipe. What should you do first?",
        a: [
          "Replace the whole pipework system straight away",
          "Listen to the report and investigate the fault",
          "Ask the customer to repair it temporarily",
          "Complete the paperwork before viewing the leak"
        ],
        correct: 1,
        feedback: "Correct. Reactive maintenance starts with listening, inspecting and identifying the fault."
      },
      {
        q: "Which action best supports environmental responsibility during painting work?",
        a: [
          "Pour unused paint into the nearest drain",
          "Store waste together to make disposal quicker",
          "Minimise waste and dispose of materials correctly",
          "Leave tins open so paint dries before storage"
        ],
        correct: 2,
        feedback: "Correct. Waste should be reduced, reused, recycled or disposed of safely."
      },
      {
        q: "Cold water from an outlet is recorded above 20°C after flushing. What should this suggest?",
        a: [
          "The outlet is working correctly and needs no action",
          "There may be a water hygiene concern to report",
          "The hot water system is operating too efficiently",
          "The tap should be used more often without reporting"
        ],
        correct: 1,
        feedback: "Correct. Cold water above expected temperature may increase hygiene risk and should be reported."
      },
      {
        q: "When using access equipment, what is most important?",
        a: [
          "Choose the quickest item available nearby",
          "Use the equipment only if another person is present",
          "Select suitable equipment and check it before use",
          "Stand on materials if the task is only brief"
        ],
        correct: 2,
        feedback: "Correct. Access equipment must be suitable, checked and used safely."
      },
      {
        q: "Why is customer sign-off important after completing a repair?",
        a: [
          "It confirms the work has been checked and accepted",
          "It removes the need to clean the work area",
          "It proves the customer watched the full repair",
          "It allows defects to be ignored after completion"
        ],
        correct: 0,
        feedback: "Correct. Sign-off helps confirm completion, quality checks and customer satisfaction."
      },
      {
        q: "Which behaviour best supports professional development as a PMO apprentice?",
        a: [
          "Avoiding feedback to show independence",
          "Using mentor feedback to improve future work",
          "Only practising tasks already completed well",
          "Waiting until EPA before reflecting on skills"
        ],
        correct: 1,
        feedback: "Correct. Reflection and mentor feedback support skill development and EPA preparation."
      }
    ];

    let currentQuestion = 0;
    let score = 0;
    let answered = false;

    function startGame() {
      document.getElementById("startScreen").style.display = "none";
      document.getElementById("quizScreen").style.display = "block";
      showQuestion();
    }

    function showQuestion() {
      answered = false;
      const q = questions[currentQuestion];
      document.getElementById("questionNumber").textContent =
        "Question " + (currentQuestion + 1) + " of " + questions.length;

      document.getElementById("progressBar").style.width =
        ((currentQuestion) / questions.length) * 100 + "%";

      document.getElementById("questionText").textContent = q.q;

      const answersDiv = document.getElementById("answers");
      answersDiv.innerHTML = "";

      q.a.forEach((answer, index) => {
        const btn = document.createElement("button");
        btn.className = "answer";
        btn.textContent = answer;
        btn.onclick = () => checkAnswer(index, btn);
        answersDiv.appendChild(btn);
      });

      document.getElementById("feedback").style.display = "none";
      document.getElementById("nextBtn").style.display = "none";
    }

    function checkAnswer(index, button) {
      if (answered) return;
      answered = true;

      const q = questions[currentQuestion];
      const allButtons = document.querySelectorAll("#answers button");
      const feedback = document.getElementById("feedback");

      allButtons.forEach((btn, i) => {
        btn.disabled = true;
        if (i === q.correct) btn.classList.add("correct");
      });

      if (index === q.correct) {
        score++;
        feedback.className = "feedback good";
        feedback.textContent = q.feedback;
      } else {
        button.classList.add("wrong");
        feedback.className = "feedback bad";
        feedback.textContent = "Not quite. " + q.feedback;
      }

      feedback.style.display = "block";
      document.getElementById("nextBtn").style.display = "block";
    }

    function nextQuestion() {
      currentQuestion++;

      if (currentQuestion < questions.length) {
        showQuestion();
      } else {
        endGame();
      }
    }

    function endGame() {
      document.getElementById("quizScreen").style.display = "none";
      document.getElementById("endScreen").style.display = "block";
      document.getElementById("progressBar").style.width = "100%";

      const percentage = Math.round((score / questions.length) * 100);
      document.getElementById("finalScore").textContent =
        score + " / " + questions.length + " (" + percentage + "%)";

      let message = "";

      if (percentage >= 85) {
        message = "Excellent result. This is the target standard for strong EPA preparation.";
      } else if (percentage >= 68) {
        message = "Good attempt. You reached a pass level, but further revision is needed to reach the 85% target.";
      } else {
        message = "Further revision is needed. Revisit the key topics and try the challenge again.";
      }

      document.getElementById("finalMessage").textContent = message;
    }

    function restartGame() {
      currentQuestion = 0;
      score = 0;
      document.getElementById("endScreen").style.display = "none";
      document.getElementById("quizScreen").style.display = "block";
      showQuestion();
    }
  </script>
</div>
