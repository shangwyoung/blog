As part of my frontend skills development, I will be building a tic tac toe game.
I wanted to learn how to use HTML and CSS preprocessors and will be using Jade and
Sass to build my project.

Let's start by putting down the structural elements of my tic tac toe board. We'll
use a 3X3 table for our board, and number each cell.

```html
#board
  table
    tr
        td#cell7
        td#cell8
        td#cell9
    tr
        td#cell4
        td#cell5
        td#cell6
    tr
        td#cell1
        td#cell2
        td#cell3
```

http://stackoverflow.com/questions/7059394/how-to-position-a-table-at-the-center-of-div-horizontally-vertically

Let's center the table, and give each td a size of 100X100 and borders to visually see the board.

```css
body
  padding: 60px

table
  margin: 0 auto

td
  height: 100px
  width: 100px

#row1, #row2
  border-bottom: solid

#cell7, #cell8, #cell4, #cell5, #cell1, #cell2
  border-right: solid
```

For each td, let's add a .sign class where the O's and X's will live in:

```html
#board
  table
    tr#row1
        td#cell7
          .sign
        td#cell8
          .sign
        td#cell9
          .sign
    tr#row2
        td#cell4
          .sign
        td#cell5
          .sign
        td#cell6
          .sign
    tr#row3
        td#cell1
          .sign
        td#cell2
          .sign
        td#cell3
          .sign
```

I found a letter o and letter x on flaticon:

http://www.flaticon.com/free-icon/letter-x_109602
http://www.flaticon.com/free-icon/circle-ring_16894

I'm thinking I can simply append the <img/> tag to the .sign div. For testing purposes,
let's try that in js:

```javascript
$(document).ready(function () {
  var X = "<img src='http://cs.oberlin.edu/~syoung/images/letter-x.svg'>";
  var O = "<img src='http://cs.oberlin.edu/~syoung/images/letter-o.svg'>";
  $("#cell5 .sign").append(X);
  $("#cell8 .sign").append(O);
});
```

That works! Let's try to implement some basic funtionalities of the board. Let's
add an event listener on the td elements. When clicked, either an O or X will be
placed inside that cell. O's and X's will alternate between clicks. Then, we check
whether there is a winning configuration. I'm just doing an exhaustive search through
all 8 possible winning configurations. If a winning configuration is found, I throw an
alert. I'll also keep track of the number of turns, so that if there's no winning config
but has taken 9 turns, it will declare a tie.

```javascript
var X = "<img src='http://cs.oberlin.edu/~syoung/images/letter-x.svg'>";
var O = "<img src='http://cs.oberlin.edu/~syoung/images/letter-o.svg'>";
var circle = true;
var board = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0];
var turns = 0;

$(document).ready(function () {
  $("td").on("click", playRound);
});

function playRound() {
  turns++;
  var cell = $(this).find(".sign");
  var id = $(this).attr("id")[4];
  if (circle) {
    cell.append(O);
    board[id] = 1;
  } else {
    cell.append(X);
    board[id] = 2;
  }
  checkForWinner();
}

function checkForWinner() {
  var win = false;
  var player = circle ? 1 : 2;

  if (board[7] == player && board[8] == player && board[9] == player ||
      board[4] == player && board[5] == player && board[6] == player ||
      board[1] == player && board[2] == player && board[3] == player ||
      board[7] == player && board[4] == player && board[1] == player ||
      board[8] == player && board[5] == player && board[2] == player ||
      board[9] == player && board[6] == player && board[3] == player ||
      board[7] == player && board[5] == player && board[3] == player ||
      board[9] == player && board[5] == player && board[1] == player) {
    win = true;
  }

  if (win) {
    alert("Player " + player + " wins!");
    resetBoard();
  } else if (turns == 9) {
    alert("It's a tie!");
    resetBoard();
  } else {
    circle = !circle;
  }
}

function resetBoard() {
  $(".sign").empty();
  board = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0];
  circle = true;
  turns = 0;
}
```

Issue: DOM not updating in real-time
---

Cool. This works! However, there's one issue: the DOM doesn't update before the
alert is prompted. Turns out, this is a common issue, where javascript code and
UI is run on the same thread, and UI is only updated once js code has finished execution:

http://stackoverflow.com/questions/5743428/javascript-progress-bar-not-updating-on-the-fly-but-all-at-once-once-process

Solution: setTimeout
---

The suggested solution is to use setTimeout to delay the js code, giving the DOM
a chance to update.

```javascript
if (win) {
    setTimeout(function() {
      alert("Player " + player + " wins!");
      resetBoard();
    }, 100);
  } else if (turns == 9) {
    setTimeout(function() {
      alert("It's a tie!");
      resetBoard();
    }, 100);
  } else {
    circle = !circle;
  }
```
This worked perfectly!

Issue: A cell can hold multiple O's and X's
---

We haven't yet disallowed a player to place multiple O's and X's on a single cell. Let's fix this.
I can simply check the value in my board array that corresponds to the cell the player
is trying to play his turn. If it's 0, then it's a value move. Otherwise, its not.
I made this change in the playRound function:

```javascript
function playRound() {
  var cell = $(this).find(".sign");
  var id = $(this).attr("id")[4];
  // check if it is a valid move
  if (board[id] == 0) {
    turns++;
    if (circle) {
      cell.append(O);
      board[id] = 1;
    } else {
      cell.append(X);
      board[id] = 2;
    }
    checkForWinner();
  }
}
```

Next, let's give the player some game options:
1. One player or Two players
2. X or O

To implement this, I'm going to have a total of three states:
state 1: How many players?
state 2: X or O?
state 3: game

I will put all three states in a "belt" with 300% width, and slide the belt
as the player selects game options. Note: to make the math easier, I'll make the
belt 400%, with no 4th state.

```html
.belt
  .state#player
    h3 How many players?
    .button.btn1#onePlayer One Player
    .button.btn1#twoPlayer Two Players
  .state#symbol
    h3 Would Player 1 like to be O or X?
    .button.btn2#O O
    .button.btn2#X X
    .button#backFrom2 Back
  .state#board
    .turn.show-turn#player1 Player 1's turn
    .turn#player2 Player 2's turn
    table
      tr#row1
          td#cell7
            .sign
          td#cell8
            .sign
          td#cell9
            .sign
      tr#row2
          td#cell4
            .sign
          td#cell5
            .sign
          td#cell6
            .sign
      tr#row3
          td#cell1
            .sign
          td#cell2
            .sign
          td#cell3
            .sign
    .button#resetBoard Reset Board
    .button#resetAll Reset All
```

```css
.belt
  width: 400% // width of the entire belt
  display: flex
  transition: transform 400ms ease-in-out

.state
  padding: 60px
  width: 25% // width relative to the belt
  text-align: center

.button
  border-radius: 5px
  border: 2px solid black
  cursor: pointer
  width: 300px
  margin: 50px auto

.stage2
  transform: translateX(-25%)
.stage3
  transform: translate(-50%)
```

```javascript
$(document).ready(function () {
  $("td").on("click", playRound);
  $('.btn1').on("click", stageTwo);
  $('.btn2').on("click", stageThree);
  $('.btn3').on("click", stageReset);
});

function stageTwo() {
  $('.belt').toggleClass('stage2');
}

function stageThree() {
  $('.belt').toggleClass('stage2');
  $('.belt').toggleClass('stage3');
}

function stageReset() {
  gameReset();
  $('.belt').toggleClass('stage3');
}

function gameReset() {
  resetBoard();
}
```

Now that we've implemented our sliding belt (which is cool), let's implement the
game options.

For number of players, I simply keep track of it in a variable.
For O's or X's, I created a new variable player1 to keep track of which symbol
player 1 is, where 1 = O and 2 = X.

tl;dr

After some refactoring, and adding the feature to display whose turn it is, here's where
I am:

<p data-height="550" data-theme-id="0" data-slug-hash="NRKPqv" data-default-tab="js,result" data-user="aaronyoung" data-embed-version="2" class="codepen">See the Pen <a href="https://codepen.io/aaronyoung/pen/NRKPqv/">Tic Tac Toe: part 1</a> by Aaron Young (<a href="http://codepen.io/aaronyoung">@aaronyoung</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Two player game is now completely functional! yay. Now let's tackle the hard part;
playing with the computer.
