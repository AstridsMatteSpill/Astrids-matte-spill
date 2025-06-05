<!DOCTYPE html>
<html lang="no">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Astrids geometriske quiz</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap');

  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: #fff;
    margin: 0; padding: 0;
    display: flex; flex-direction: column; align-items: center;
    min-height: 100vh;
    color: #333;
  }
  header {
    margin-top: 25px;
    font-family: 'Montserrat', sans-serif;
    font-weight: 700;
    font-size: 1.5rem;
    color: #4b0082; /* mørk lilla */
    text-align: center;
  }
  header small {
    font-weight: normal;
    font-size: 1rem;
    color: #666;
    display: block;
    margin-top: 3px;
  }

  #score {
    position: fixed;
    top: 15px;
    left: 15px;
    font-weight: bold;
    background: #9c27b0;
    color: white;
    padding: 8px 12px;
    border-radius: 6px;
    box-shadow: 0 2px 6px rgba(156,39,176,0.5);
    font-size: 1rem;
    z-index: 10;
  }

  #quiz-container {
    background: #fff;
    width: 90%;
    max-width: 600px;
    border-radius: 10px;
    padding: 30px 30px 50px;
    box-sizing: border-box;
    color: #333;
    box-shadow: 0 4px 15px rgba(156,39,176,0.15);
    margin-top: 70px;
    text-align: center;
  }

  #question {
    font-size: 1.4rem;
    margin-bottom: 15px;
    font-weight: 600;
  }

  #shape-box {
    margin: 20px auto 25px;
    width: 160px;
    height: 160px;
    position: relative;
  }

  /* Lilla figurer med fin kant */
  .square {
    width: 140px;
    height: 140px;
    background: #9c27b0;
    border-radius: 6px;
    margin: 0 auto;
    box-shadow: 0 0 12px #7b1fa2aa;
  }
  .rectangle {
    width: 180px;
    height: 100px;
    background: #9c27b0;
    border-radius: 6px;
    margin: 0 auto;
    box-shadow: 0 0 12px #7b1fa2aa;
  }
  .circle {
    width: 140px;
    height: 140px;
    background: #9c27b0;
    border-radius: 50%;
    margin: 0 auto;
    box-shadow: 0 0 12px #7b1fa2aa;
  }

  #answer-container {
    margin-top: 20px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    gap: 10px;
  }

  #answer-input {
    width: 130px;
    font-size: 1.2rem;
    padding: 12px;
    border: none;
    border-radius: 8px;
    background: #000;
    color: #fff;
    text-align: center;
    outline-offset: 2px;
    outline-color: #9c27b0;
    transition: outline-color 0.3s ease;
  }
  #answer-input:focus {
    outline-color: #7b1fa2;
  }

  #check-btn {
    background: #9c27b0;
    border: none;
    border-radius: 8px;
    font-weight: 700;
    cursor: pointer;
    padding: 12px 24px;
    color: white;
    font-size: 1.2rem;
    box-shadow: 0 4px 12px #7b1fa2cc;
    transition: background-color 0.3s ease;
  }
  #check-btn:hover:not(:disabled) {
    background: #7b1fa2;
  }
  #check-btn:disabled {
    opacity: 0.5;
    cursor: default;
  }

  #nav-buttons {
    margin-top: 30px;
    display: flex;
    justify-content: space-between;
  }
  button.nav-btn {
    background: #eee;
    border: none;
    border-radius: 8px;
    padding: 12px 24px;
    font-weight: 700;
    cursor: pointer;
    color: #9c27b0;
    transition: background-color 0.3s ease;
    font-size: 1rem;
  }
  button.nav-btn:disabled {
    opacity: 0.5;
    cursor: default;
  }
  button.nav-btn:hover:not(:disabled) {
    background: #d1a8e0;
  }

  #result-msg {
    margin-top: 18px;
    font-weight: 700;
    min-height: 26px;
    font-size: 1.1rem;
    color: #4b0082;
  }

  #progress {
    margin-top: 15px;
    font-size: 1rem;
    color: #555;
  }
</style>
</head>
<body>

<header>
  Astrids geometriske quiz
  <small>laget av Julian</small>
</header>

<div id="score">Poeng: 0 / 20</div>

<div id="quiz-container">
  <div id="question"></div>
  <div id="shape-box"></div>

  <div id="answer-container">
    <input type="number" id="answer-input" placeholder="Skriv svar her" />
    <button id="check-btn" disabled>Sjekk svar</button>
  </div>

  <div id="result-msg"></div>

  <div id="nav-buttons">
    <button id="prev-btn" class="nav-btn" disabled>Forrige</button>
    <button id="next-btn" class="nav-btn" disabled>Neste</button>
  </div>

  <div id="progress"></div>
</div>

<script>
  // Funksjoner for areal og omkrets:
  function arealKvadrat(s) { return s*s; }
  function omkretsKvadrat(s) { return 4*s; }
  function arealRektangel(l,b) { return l*b; }
  function omkretsRektangel(l,b) { return 2*(l+b); }
  function arealSirkel(r) { return Math.round(Math.PI*r*r); }
  function omkretsSirkel(r) { return Math.round(2*Math.PI*r); }

  // Oppgaver - lettere tall, 20 oppgaver, både areal og omkrets
  const questions = [
    {shape:'square',type:'areal',values:[4], question:'Hva er arealet av et kvadrat med side 4?', answer:arealKvadrat(4)},
    {shape:'square',type:'omkrets',values:[6], question:'Hva er omkretsen av et kvadrat med side 6?', answer:omkretsKvadrat(6)},
    {shape:'rectangle',type:'areal',values:[5,3], question:'Hva er arealet av et rektangel med lengde 5 og bredde 3?', answer:arealRektangel(5,3)},
    {shape:'rectangle',type:'omkrets',values:[7,2], question:'Hva er omkretsen av et rektangel med lengde 7 og bredde 2?', answer:omkretsRektangel(7,2)},
    {shape:'circle',type:'areal',values:[3], question:'Hva er arealet av en sirkel med radius 3? (avrundet)', answer:arealSirkel(3)},
    {shape:'circle',type:'omkrets',values:[4], question:'Hva er omkretsen av en sirkel med radius 4? (avrundet)', answer:omkretsSirkel(4)},
    {shape:'square',type:'areal',values:[7], question:'Hva er arealet av et kvadrat med side 7?', answer:arealKvadrat(7)},
    {shape:'square',type:'omkrets',values:[9], question:'Hva er omkretsen av et kvadrat med side 9?', answer:omkretsKvadrat(9)},
    {shape:'rectangle',type:'areal',values:[6,8], question:'Hva er arealet av et rektangel med lengde 6 og bredde 8?', answer:arealRektangel(6,8)},
    {shape:'rectangle',type:'omkrets',values:[5,5], question:'Hva er omkretsen av et rektangel med lengde 5 og bredde 5?', answer:omkretsRektangel(5,5)},
    {shape:'circle',type:'areal',values:[5], question:'Hva er arealet av en sirkel med radius 5? (avrundet)', answer:arealSirkel(5)},
    {shape:'circle',type:'omkrets',values:[2], question:'Hva er omkretsen av en sirkel med radius 2? (avrundet)', answer:omkretsSirkel(2)},
    {shape:'square',type:'areal',values:[10], question:'Hva er arealet av et kvadrat med side 10?', answer:arealKvadrat(10)},
    {shape:'square',type:'omkrets',values:[3], question:'Hva er omkretsen av et kvadrat med side 3?', answer:omkretsKvadrat(3)},
    {shape:'rectangle',type:'areal',values:[9,4], question:'Hva er arealet av et rektangel med lengde 9 og bredde 4?', answer:arealRektangel(9,4)},
    {shape:'rectangle',type:'omkrets',values:[10,7], question:'Hva er omkretsen av et rektangel med lengde 10 og bredde 7?', answer:omkretsRektangel(10,7)},
    {shape:'circle',type:'areal',values:[6], question:'Hva er arealet av en sirkel med radius 6? (avrundet)', answer:arealSirkel(6)},
    {shape:'circle',type:'omkrets',values:[7], question:'Hva er omkretsen av en sirkel med radius 7? (avrundet)', answer:omkretsSirkel(7)},
    {shape:'square',type:'areal',values:[8], question:'Hva er arealet av et kvadrat med side 8?', answer:arealKvadrat(8)},
    {shape:'square',type:'omkrets',values:[5], question:'Hva er omkretsen av et kvadrat med side 5?', answer:omkretsKvadrat(5)},
  ];

  let currentIndex = 0;
  let score = 0;
  let answered = Array(questions.length).fill(false);

  const questionEl = document.getElementById('question');
  const shapeBox = document.getElementById('shape-box');
  const answerInput = document.getElementById('answer-input');
  const checkBtn = document.getElementById('check-btn');
  const resultMsg = document.getElementById('result-msg');
  const prevBtn = document.getElementById('prev-btn');
  const nextBtn = document.getElementById('next-btn');
  const scoreEl = document.getElementById('score');
  const progressEl = document.getElementById('progress');

  function renderShape(shape) {
    shapeBox.innerHTML = '';
    const div = document.createElement('div');
    if(shape === 'square') div.className = 'square';
    else if(shape === 'rectangle') div.className = 'rectangle';
    else if(shape === 'circle') div.className = 'circle';
    shapeBox.appendChild(div);
  }

  function loadQuestion(index) {
    const q = questions[index];
    questionEl.textContent = q.question;
    renderShape(q.shape);
    answerInput.value = '';
    answerInput.disabled = false;
    checkBtn.disabled = true;
    resultMsg.textContent = '';
    updateNavButtons();
    updateProgress();
  }

  function updateScore() {
    scoreEl.textContent = `Poeng: ${score} / ${questions.length}`;
  }

  function updateNavButtons() {
    prevBtn.disabled = currentIndex === 0;
    nextBtn.disabled = currentIndex === questions.length -1;
  }

  function updateProgress() {
    progressEl.textContent = `Oppgave ${currentIndex+1} av ${questions.length}`;
  }

  answerInput.addEventListener('input', () => {
    checkBtn.disabled = answerInput.value.trim() === '';
    resultMsg.textContent = '';
  });

  checkBtn.addEventListener('click', () => {
    const userAns = Number(answerInput.value.trim());
    const correctAns = questions[currentIndex].answer;

    if(userAns === correctAns) {
      if(!answered[currentIndex]){
        score++;
        updateScore();
      }
      answered[currentIndex] = true;
      resultMsg.style.color = '#4caf50'; // grønn
      resultMsg.textContent = 'Riktig! 👍';
      answerInput.disabled = true;
      checkBtn.disabled = true;
    } else {
      resultMsg.style.color = '#e53935'; // rød
      resultMsg.textContent = 'Feil, prøv igjen.';
    }
  });

  prevBtn.addEventListener('click', () => {
    if(currentIndex > 0){
      currentIndex--;
      loadQuestion(currentIndex);
    }
  });

  nextBtn.addEventListener('click', () => {
    if(currentIndex < questions.length -1){
      currentIndex++;
      loadQuestion(currentIndex);
    }
  });

  // Start quizen
  updateScore();
  loadQuestion(currentIndex);
</script>

</body>
</html>
