# memory-spiel
Eine Memorie spiel
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Memory Spiel</title>
<style>
body {
  font-family: -apple-system, BlinkMacSystemFont, sans-serif;
  background: #f2f2f2;
  text-align: center;
}

h1 {
  margin-top: 10px;
}

.game {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 12px;
  max-width: 420px;
  margin: 20px auto;
}

.card {
  background: #1e90ff;
  color: white;
  font-size: 40px;
  border-radius: 12px;
  height: 90px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  user-select: none;
}

.card.open, .card.matched {
  background: white;
  color: black;
  cursor: default;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 10px;
  border: none;
  background: #1e90ff;
  color: white;
}
</style>
</head>
<body>

<h1>Memory</h1>
<p>Zwei Leitmotive – zufällige Paare</p>

<div class="game" id="game"></div>

<button onclick="startGame()">Neu starten</button>

<script>
const motifA = ["🐶","🐱","🐭","🐰","🦊","🐼","🐸","🐵"];
const motifB = ["🐾","🦴","🧶","🥕","🌲","🎋","🍃","🍌"];

let firstCard = null;
let lock = false;

function shuffle(array) {
  return array.sort(() => Math.random() - 0.5);
}

function startGame() {
  const game = document.getElementById("game");
  game.innerHTML = "";
  firstCard = null;
  lock = false;

  let pairs = [];
  for (let i = 0; i < 8; i++) {
    pairs.push(
      { id: i, symbol: motifA[i] },
      { id: i, symbol: motifB[i] }
    );
  }

  shuffle(pairs);

  pairs.forEach(cardData => {
    const card = document.createElement("div");
    card.className = "card";
    card.dataset.id = cardData.id;
    card.textContent = "❓";

    card.onclick = () => {
      if (lock || card.classList.contains("open")) return;

      card.classList.add("open");
      card.textContent = cardData.symbol;

      if (!firstCard) {
        firstCard = card;
      } else {
        if (firstCard.dataset.id === card.dataset.id) {
          firstCard.classList.add("matched");
          card.classList.add("matched");
          firstCard = null;
        } else {
          lock = true;
          setTimeout(() => {
            firstCard.classList.remove("open");
            card.classList.remove("open");
            firstCard.textContent = "❓";
            card.textContent = "❓";
            firstCard = null;
            lock = false;
          }, 800);
        }
      }
    };

    game.appendChild(card);
  });
}

startGame();
</script>

</body>
</html>
