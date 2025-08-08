const goalInput = document.getElementById('goalInput');
const setGoalBtn = document.getElementById('setGoalBtn');
const intakeInput = document.getElementById('intakeInput');
const addIntakeBtn = document.getElementById('addIntakeBtn');
const progressFill = document.getElementById('progressFill');
const statusText = document.getElementById('statusText');
const themeSelect = document.getElementById('themeSelect');

let goal = localStorage.getItem('goal') ? parseInt(localStorage.getItem('goal')) : 2000;
let intake = localStorage.getItem('intake') ? parseInt(localStorage.getItem('intake')) : 0;
let lastDate = localStorage.getItem('lastDate') || null;

// Nollställ vid midnatt (om datum ändrats)
function resetIfNewDay() {
  const today = new Date().toISOString().slice(0, 10);
  if (lastDate !== today) {
    intake = 0;
    lastDate = today;
    localStorage.setItem('intake', intake);
    localStorage.setItem('lastDate', lastDate);
  }
}

// Uppdatera UI och progress
function updateUI() {
  resetIfNewDay();

  goalInput.value = goal;
  themeSelect.value = localStorage.getItem('theme') || 'light';
  applyTheme(themeSelect.value);

  progressFill.style.width = Math.min((intake / goal) * 100, 100) + '%';

  let remaining = goal - intake;
  if (remaining <= 0) {
    statusText.textContent = `Du har nått ditt mål! 🎉`;
  } else {
    statusText.textContent = `Du har ${remaining} ml kvar att dricka idag.`;
  }
}

// Tema-funktion
function applyTheme(theme) {
  if (theme === 'dark') {
    document.body.classList.add('dark');
  } else {
    document.body.classList.remove('dark');
  }
  localStorage.setItem('theme', theme);
}

// Händelser
setGoalBtn.addEventListener('click', () => {
  const val = parseInt(goalInput.value);
  if (!isNaN(val) && val > 0) {
    goal = val;
    localStorage.setItem('goal', goal);
    updateUI();
  }
});

addIntakeBtn.addEventListener('click', () => {
  const val = parseInt(intakeInput.value);
  if (!isNaN(val) && val > 0) {
    intake += val;
    localStorage.setItem('intake', intake);
    updateUI();
    intakeInput.value = '';
  }
});

themeSelect.addEventListener('change', () => {
  applyTheme(themeSelect.value);
});

updateUI();
