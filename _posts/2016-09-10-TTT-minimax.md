---
# post information
layout: post
title:  "Tic Tac Toe: Minimax Algorithm"
date:   2016-09-10 11:00:00 -0500
# author information
author: Aaron Young
profilePic: profile-sm.jpg
about: /blog/about
# comments
comments: true
# cover photo
cover-photo: "assets/TTT-minimax.png"
# category and tags
category: programming
tags: javascript freecodecamp
---

One of the features I wanted to have in my Tic Tac Toe game was an unbeatable computer, but I wasn't entirely sure how I was going to implement it. After some googling, I came across a very well written article on this exact problem:

[Tic Tac Toe: Understanding The Minimax Algorithm][1]

The article explains the Minimax algorithm through several examples and diagrams in a very intuitive way. I definitely would recommend the article as it helped me understand the Minimax algorithm, and made implementing my own Tic Tac Toe algorithm in Javascript a breeze.

Here is my Javascript version of the algorithm, with pieces of it taken out in order to focus on the relevant parts:

```javascript
function playRound() {
  .
  .
  .
    // if it's a single player game
    if (players == 1) {
      // computer's turn
      if (gameWin(board) == -1) { // if game is not over
        computerMove(board); // play computer's move
      }
    }
  }
}
```

The ```computerMove``` function calls the minimax function for every possible move, and plays the one that yields the max score.

```javascript
function computerMove(game) {
  moves = []; // array of moves
  scores = []; // array of scores

  // for each cell in the board
  for (var i = 1; i < game.length; i++) {
    // if the cell is available
    if (game[i] == 0) {
      // add move to array of moves
      moves.push(i);
      // add the corresponding score
      // by calling minimax with the computer's
      // potential move. Where player1 is the nextTurnPlayer
      scores.push(minimax(game.slice(0, i).concat([computer], game.slice(i + 1, game.length)), player1));
    }
  }
  // find the move corresponding to the max score
  var cellNum = moves[maxIndex(scores)];
  // play the move
  board[cellNum] = computer;
  // check for a winner
  checkForWinner();
  return cellNum;
}

// Stops the game if there's a winner
function checkForWinner() {
  .
  .
  .
}
```

Here is the actual minimax function that will find the ultimate score for each possible move:

```javascript
function minimax(game, turnPlayer) {
  var winner = gameWin(game);
  // if there is a winner
  if (winner >= 0) {
    return score(game); // return the score
  }

  // otherwise, the game continues
  var scores = [];
  var moves = [];
  var nextTurnPlayer = (turnPlayer == 1) ? 2 : 1;

  // for each cell in the board
  for (var i = 1; i < game.length; i++) {
    // if the cell is available
    if (game[i] == 0) {
      // add move to array of moves
      moves.push(i);
      // add the corresponding score
      // by calling minimax with the turnPlayer's
      // potential move. Where nextTurnPlayer is the next turnPlayer
      scores.push(minimax(game.slice(0, i).concat([turnPlayer], game.slice(i + 1, game.length)), nextTurnPlayer));
    }
  }

  // if it's the computer's turn
  if (turnPlayer == computer) {
    return max(scores); // return the max score
  } else { // if it's player1's turn
    return min(scores); // return the min score
  }
}
```

Here is the function that scores the game, along with other helper functions:

```javascript
function score(game) {
  // if the winner is the computer
  if (gameWin(game, computer) == computer) {
    return 10;
  // if the winner is player1
  } else if (gameWin(game, player1) == player1) {
    return -10;
  } else { // if there's no winner
    return 0;
  }
}

// returned the max value in array
function max(A) {
.
.
.
}

// returned the min value in array
function min(A) {
.
.
.
}

// returned the index of the max value in array
function maxIndex(A) {
.
.
.
}

// takes in a board, and returns the winning symbol
// returns -1 if no winner
// returns 0 if tied
function gameWin(board) {
  .
  .
  .
}
```

Refactor? Shhh...
---
---

If you noticed that the ```computerMove``` and ```minimax``` functions look very similar, you're right. I probably could/should refactor the code to eliminate repeated code, but that's for another day.

Here is the full game, have fun not winning!

<p data-height="584" data-theme-id="0" data-slug-hash="kXKqBZ" data-default-tab="result" data-user="aaronyoung" data-embed-version="2" class="codepen">See the Pen <a href="https://codepen.io/aaronyoung/pen/kXKqBZ/">Tic Tac Toe</a> by Aaron Young (<a href="http://codepen.io/aaronyoung">@aaronyoung</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

[1]: http://neverstopbuilding.com/minimax
