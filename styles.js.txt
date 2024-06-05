document.addEventListener('DOMContentLoaded', function() {
  document.getElementById('languageSelector').addEventListener('change', function() {
    const language = this.value;
    setLanguage(language);
  });


  loadHistory();
});


function setLanguage(language) {
  const translations = {
    en: {
      welcomeMessage: "Welcome to CXI Chess ELO",
      player1Label: "Player 1 Rating:",
      player2Label: "Player 2 Rating:",
      resultLabel: "Result:",
      kFactor1Label: "K Factor Player 1:",
      kFactor2Label: "K Factor Player 2:",
      calculateButton: "Calculate New Ratings",
      newRatingsTitle: "New Ratings",
      historyTitle: "Match History",
      clearHistoryButton: "Clear History",
      player1History: "Player 1",
      player1RatingBefore: "Player 1 Rating (Before)",
      player2History: "Player 2",
      player2RatingBefore: "Player 2 Rating (Before)",
      resultHistory: "Result",
      player1RatingAfter: "Player 1 Rating (After)",
      player2RatingAfter: "Player 2 Rating (After)",
      placeholders: {
        player1: "Enter rating",
        player2: "Enter rating"
      },
      kOptions: [
        "K40 - Under 18 years or fewer than 30 games",
        "K20 - Below 2400 (or Rapid and Blitz rating)",
        "K10 - Over 2400"
      ],
      resultOptions: {
        win: "Player 1 Wins",
        draw: "Draw",
        lose: "Player 2 Wins"
      }
    },
    pt: {
      welcomeMessage: "Bem vindo ao CXI Chess ELO",
      player1Label: "Rating do Jogador 1:",
      player2Label: "Rating do Jogador 2:",
      resultLabel: "Resultado:",
      kFactor1Label: "Fator K Jogador 1:",
      kFactor2Label: "Fator K Jogador 2:",
      calculateButton: "Calcular Novos Ratings",
      newRatingsTitle: "Novos Ratings",
      historyTitle: "Histórico de Partidas",
      clearHistoryButton: "Limpar Histórico",
      player1History: "Jogador 1",
      player1RatingBefore: "Rating do Jogador 1 (Antes)",
      player2History: "Jogador 2",
      player2RatingBefore: "Rating do Jogador 2 (Antes)",
      resultHistory: "Resultado",
      player1RatingAfter: "Rating do Jogador 1 (Depois)",
      player2RatingAfter: "Rating do Jogador 2 (Depois)",
      placeholders: {
        player1: "Digite o rating",
        player2: "Digite o rating"
      },
      kOptions: [
        "K40 - Menos de 18 anos ou menos de 30 partidas",
        "K20 - Menos de 2400 (ou Rápido e Blitz qual rating)",
        "K10 - mais de 2400"
      ],
      resultOptions: {
        win: "Jogador 1 Vence",
        draw: "Empate",
        lose: "Jogador 2 Vence"
      }
    },
    es: {
      welcomeMessage: "Bienvenido a CXI Chess ELO",
      player1Label: "Rating del Jugador 1:",
      player2Label: "Rating del Jugador 2:",
      resultLabel: "Resultado:",
      kFactor1Label: "Factor K Jugador 1:",
      kFactor2Label: "Factor K Jugador 2:",
      calculateButton: "Calcular Nuevos Ratings",
      newRatingsTitle: "Nuevos Ratings",
      historyTitle: "Historial de Partidas",
      clearHistoryButton: "Limpiar Historial",
      player1History: "Jugador 1",
      player1RatingBefore: "Rating del Jugador 1 (Antes)",
      player2History: "Jugador 2",
      player2RatingBefore: "Rating del Jugador 2 (Antes)",
      resultHistory: "Resultado",
      player1RatingAfter: "Rating del Jugador 1 (Después)",
      player2RatingAfter: "Rating del Jugador 2 (Después)",
      placeholders: {
        player1: "Introduzca el rating",
        player2: "Introduzca el rating"
      },
      kOptions: [
        "K40 - Menos de 18 años o menos de 30 partidas",
        "K20 - Menos de 2400 (o Rápido y Blitz rating)",
        "K10 - más de 2400"
      ],
      resultOptions: {
        win: "Jugador 1 Gana",
        draw: "Empate",
        lose: "Jugador 2 Gana"
      }
    }
  };


  const translation = translations[language];
  document.getElementById('welcomeMessage').textContent = translation.welcomeMessage;
  document.getElementById('player1Label').textContent = translation.player1Label;
  document.getElementById('player2Label').textContent = translation.player2Label;
  document.getElementById('resultLabel').textContent = translation.resultLabel;
  document.getElementById('kFactor1Label').textContent = translation.kFactor1Label;
  document.getElementById('kFactor2Label').textContent = translation.kFactor2Label;
  document.getElementById('calculateButton').textContent = translation.calculateButton;
  document.getElementById('newRatingsTitle').textContent = translation.newRatingsTitle;
  document.getElementById('historyTitle').textContent = translation.historyTitle;
  document.getElementById('clearHistoryButton').textContent = translation.clearHistoryButton;
  document.getElementById('player1History').textContent = translation.player1History;
  document.getElementById('player1RatingBefore').textContent = translation.player1RatingBefore;
  document.getElementById('player2History').textContent = translation.player2History;
  document.getElementById('player2RatingBefore').textContent = translation.player2RatingBefore;
  document.getElementById('resultHistory').textContent = translation.resultHistory;
  document.getElementById('player1RatingAfter').textContent = translation.player1RatingAfter;
  document.getElementById('player2RatingAfter').textContent = translation.player2RatingAfter;


  // Update placeholders
  document.getElementById('player1').placeholder = translation.placeholders.player1;
  document.getElementById('player2').placeholder = translation.placeholders.player2;


  // Update K factor options
  const kFactor1 = document.getElementById('kFactor1');
  const kFactor2 = document.getElementById('kFactor2');
  kFactor1.innerHTML = ''; // Clear existing options
  kFactor2.innerHTML = ''; // Clear existing options
  translation.kOptions.forEach(optionText => {
    const option1 = new Option(optionText, optionText.match(/\d+/)[0]);
    const option2 = new Option(optionText, optionText.match(/\d+/)[0]);
    kFactor1.add(option1);
    kFactor2.add(option2);
  });


  // Update result options
  const resultSelect = document.getElementById('result');
  resultSelect.innerHTML = ''; // Clear existing options
  for (const [value, text] of Object.entries(translation.resultOptions)) {
    const option = new Option(text, value);
    resultSelect.add(option);
  }
}


function calculateRating() {
  const player1Rating = parseInt(document.getElementById('player1').value);
  const player2Rating = parseInt(document.getElementById('player2').value);
  const result = document.getElementById('result').value;
  const kFactor1 = parseInt(document.getElementById('kFactor1').value);
  const kFactor2 = parseInt(document.getElementById('kFactor2').value);
  
  if (isNaN(player1Rating) || isNaN(player2Rating) || isNaN(kFactor1) || isNaN(kFactor2)) {
    alert('Please enter valid numbers for all fields.');
    return;
  }
  if (player1Rating < 0 || player2Rating < 0 || kFactor1 <= 0 || kFactor2 <= 0) {
    alert('Please enter positive numbers for ratings and K factors.');
    return;
  }
  
  let player1Score;
  let player2Score;
  
  if (result === 'win') {
    player1Score = 1;
    player2Score = 0;
  } else if (result === 'draw') {
    player1Score = 0.5;
    player2Score = 0.5;
  } else {
    player1Score = 0;
    player2Score = 1;
  }
  
  const newRating1 = calculateElo(player1Rating, player2Rating, player1Score, kFactor1);
  const newRating2 = calculateElo(player2Rating, player1Rating, player2Score, kFactor2);
  
  document.getElementById('newRating1').textContent = `Player 1: ${newRating1.toFixed(2)}`;
  document.getElementById('newRating2').textContent = `Player 2: ${newRating2.toFixed(2)}`;
  
  addMatchToHistory('Player 1', 'Player 2', player1Rating, player2Rating, result, newRating1, newRating2);
  saveHistory();
  
  document.getElementById('resultSection').style.display = 'block';
  document.getElementById('historySection').style.display = 'block';
}


function calculateElo(playerRating, opponentRating, score, k) {
  const expectedScore = 1 / (1 + Math.pow(10, (opponentRating - playerRating) / 400));
  return playerRating + k * (score - expectedScore);
}


function addMatchToHistory(player1, player2, player1RatingBefore, player2RatingBefore, result, player1RatingAfter, player2RatingAfter) {
  const historyTable = document.getElementById('historyTable').getElementsByTagName('tbody')[0];
  const newRow = historyTable.insertRow();


  newRow.insertCell(0).textContent = player1;
  newRow.insertCell(1).textContent = player1RatingBefore.toFixed(2);
  newRow.insertCell(2).textContent = player2;
  newRow.insertCell(3).textContent = player2RatingBefore.toFixed(2);
  newRow.insertCell(4).textContent = result;
  newRow.insertCell(5).textContent = player1RatingAfter.toFixed(2);
  newRow.insertCell(6).textContent = player2RatingAfter.toFixed(2);
}


function saveHistory() {
  const historyTable = document.getElementById('historyTable').getElementsByTagName('tbody')[0];
  const rows = historyTable.getElementsByTagName('tr');
  const history = [];
  
  for (let i = 0; i < rows.length; i++) {
    const cells = rows[i].getElementsByTagName('td');
    const match = {
      player1: cells[0].textContent,
      player1RatingBefore: cells[1].textContent,
      player2: cells[2].textContent,
      player2RatingBefore: cells[3].textContent,
      result: cells[4].textContent,
      player1RatingAfter: cells[5].textContent,
      player2RatingAfter: cells[6].textContent
    };
    history.push(match);
  }
  
  localStorage.setItem('matchHistory', JSON.stringify(history));
}


function loadHistory() {
  const history = JSON.parse(localStorage.getItem('matchHistory')) || [];
  for (const match of history) {
    addMatchToHistory(match.player1, match.player2, parseFloat(match.player1RatingBefore), parseFloat(match.player2RatingBefore), match.result, parseFloat(match.player1RatingAfter), parseFloat(match.player2RatingAfter));
  }
}


function clearHistory() {
  localStorage.removeItem('matchHistory');
  const historyTable = document.getElementById('historyTable').getElementsByTagName('tbody')[0];
  while (historyTable.rows.length > 0) {
    historyTable.deleteRow(0);
  }
}