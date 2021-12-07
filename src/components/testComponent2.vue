<template>
  <div class="counter">
    <textarea v-model="message" placeholder="add multiple lines"></textarea>
    <br />
    <button @click="updateData">Update</button>
    <br/>
    <span>Output:</span>
    <p style="white-space: pre-line;">{{ output }}</p>
    
  </div>
</template>

<script>

export default {
  name: 'testComponent2',
  data: function() {
    return {
      message: "",
      output: ""
    }
  },
  methods:
  {
    updateData: function () {

      var bingo = 
      {
        balls : [],
        winningboard: null
      }

     var inputlines = this.message.split('\n\n');
     bingo.balls = createBallArray(inputlines.shift().split(','));
     bingo.boards = inputlines.reduce(parseBoard,bingo)
    
     console.log("score")
  console.log("number " +bingo.balls.indexOf( bingo.winningboard.firstwinBall) + " drawOrder "+ bingo.winningboard.firstwinBall)
  console.log(bingo.winningboard.score)
  console.log(bingo.balls.indexOf( bingo.winningboard.firstwinBall)*bingo.winningboard.score)
 
    }
  }
}

function parseBoard(bingo, boardtext)
{
  var board = 
  {
    grid:        [],
    columnWin:    [],
    rowWin:       [],
    hitorder:     [],
    firstwinBall: 0,
    
    score:        0
  }
  console.log("start")
  
  console.log(bingo)
  var rows = boardtext.split('\n');
  for (let row = 0; row < rows.length; row++) {
    const rowText = rows[row];
    var columns = rowText.split(' ').filter( (data) => {return data !== ''} );
    var cols = [];  
    for (let col = 0; col < columns.length; col++) {
      const spotNumber = parseInt( columns[col]);
      
      cols[col] = spotNumber
      board.hitorder.push(parseInt(bingo.balls[spotNumber] ));
      if(!board.rowWin[row] || (bingo.balls[spotNumber] &&board.rowWin[row] < bingo.balls[spotNumber]))
      {
        board.rowWin[row] = bingo.balls[spotNumber]
      }
      if(!board.columnWin[col] || (bingo.balls[spotNumber] && board.columnWin[col]  < bingo.balls[spotNumber]))
      {
        board.columnWin[col] = bingo.balls[spotNumber]
      }
    }
    board.grid[row]=cols;
  }
  board.hitorder.sort((a,b) => {return parseInt(a)-parseInt(b)});
  var bestrow = board.rowWin.sort()[0];
  var bestcol = board.columnWin.sort()[0]
  board.firstwinBall = bestrow < bestcol ? bestrow : bestcol
  var winhit = board.hitorder.indexOf(board.firstwinBall)
  board.score = board.hitorder.slice(winhit+1).reduce((total,num) => {
    return total += bingo.balls.indexOf(num)},0)
  if(bingo.winningboard)
    bingo.winningboard = bingo.winningboard.firstwinBall > board.firstwinBall ? bingo.winningboard : board
  else
    bingo.winningboard = board
  
  return bingo;
}

function createBallArray(ballsline)
{
  var balls = [];
  for (let index = 0; index < ballsline.length; index++) {
    const element = ballsline[index];
    balls[parseInt(element)] = index;
    console.log("draworder: " + index + " number " +parseInt(element))
  
  }
  return balls;
}

/*
function reduceAoCLine(total, line)
{
  if(total ===null)
    return mapAoCLine(line);
  var component = mapAoCLine(line);
var newtotal= new Array();
  for (let index = 0; index < component.length; index++) {
    var element = parseInt(component[index]);
    var totalInt = parseInt(total[index])
    newtotal[index] = element+ totalInt; 
  }
  return newtotal;
}*/
</script>

