JavaScript for simon game follows


All the globally declared variables are in in the section below for the game; 

let order=[]; The empty array where random values between 1&4 will be pushed green =1, red = 2,
yellow = 3 and blue =4.

let playerOrder=[];empty array were the players choices are added to. Numbers one thru four are added to mimic the sequence flashed by the computer.
the goal is for playerorder to match order each round.

let flash; number of flashes in the game
let turn; stores what turn the game is on.
let good; true/false, did the player match the sequence

let compTurn; true/false , computers turn?
let intervalId;
let strict = false; tracks if strict was checked
let noise = true; 
let on = false; has the program been turned on
let win; tracks where the player won or not.

Below the elments that will be referenced are being assigned variables.
let turnCounter= document.querySelector('#turn');
let topLeft= document.querySelector('#topLeft');
let topRight= document.querySelector('#topRight')
let strictButton= document.querySelector('#strict');
let onButton= document.querySelector('#on');
let startButton= document.querySelector('#start');

Event listener to see when the strict button is checked and set the "strict" variable to reflect the checkboxes current state.
strictButton.addEventListener('change',(event)=>{
 if(strictButton.checked== true){
     strict = true;
 } else{
     strict = false;
 }
});
Event listener to see when the "On" button is checked and set the strict variable to reflect the checkboxes current state. Updates the innerHTML to "on" for the turn counter defined above if it is checked.
onButton.addEventListener('click',(event)=>{
    if(onButton.checked == true){
        on = true;
        turnCounter.innerHTML="on"; sets display to on.
    } else {
        on = false;
        turnCounter.innerHTML= ""; resets turn counter
        clearColor(); clear colors,reset to original colors
        clearInterval(intervalId);
    }
});
add click properties to the "start button"
startButton.addEventListener('click',(event)=>{
    if(on ||win){
        play();
    }
});

function play(){
    setting the intial states of the below variables for the start of the game.
    win = false;
    order = [];
    playerOrder = [];
    flash = 0;
    intervalId = 0;
    turn = 1;
    turnCounter.innerHTML = 1; displays the 1st round.
    good = true; player hasn't iput a wrong sequence yet.

    the for loop below populates the "order" array by selecting randomly one of the 4 colors each time through the loop.
    for(let i=0;i<20;i++){
        order.push(Math.floor(Math.random()*4)+1);
    }
    compTurn=true; Sets the boolean variable for the computer to true.
    intervalId = setInterval(gameTurn, 800); Runs the function "gameTurn every 800 milliseconds. flashes the colors.
}
function gameTurn(){
    on=false; setting this state so the player can't click the color buttons while the computer is going through it's sequence.

    If the numner of flashes equals the turn, then the computer has completed its turn.
    if(flash == turn ){
        clearInterval(intervalId); stops the game (done flashing lights.)
        compTurn=false; its the players turn
        clearColor(); resets the colors to original 
        on=true; when on is true the player can click the buttons.
    }

    if (compTurn){
        clearColor(); reset colors

        Set time out performs an operation once after the milliseconds called at the end of the statement.
        This operation checks to see the numbers stored in the array, for the first if statement, if the number in the array is a 1 it will run the "one function".
        The same logic applies for the remaining 3 numbers if it is not a one.
        Then the flash variable is increased by one.
        setTimeout(()=>{
            if(order[flash]==1) {
                one()
            } ;
            if(order[flash]==2) {
                two();
            }    
            if(order[flash]==3) {
                three();
            }    
            if(order[flash]==4) {
                four();
            }    
            flash++;
        },400)
    }
}
Flashes one and plays the associated sound if "one()" is called, the same logic applies for functions two(), three() and four() if they are called.
function one(){
    if(noise) {
        let audio = document.querySelector('#clip1');
        audio.play();        
    }
    noise = true;
    topLeft.style.backgroundColor="lightgreen";
}

function two(){
    if(noise) {
        let audio = document.querySelector('#clip2');
        audio.play();
    }
    noise = true;
    topRight.style.backgroundColor="tomato";
}
function three(){
    if(noise) {
        let audio = document.querySelector('#clip3');
        audio.play();
    }
    noise = true;
    bottomLeft.style.backgroundColor="yellow";
}
function four(){
    if(noise) {
        let audio = document.querySelector('#clip4');
        audio.play();
    }
    noise = true;
    bottomRight.style.backgroundColor="lightskyblue";
}


Below the is the defined function to reset the colors to the original state.
function clearColor(){
    topLeft.style.backgroundColor="rgb(2, 99, 2)";
    topRight.style.backgroundColor="rgb(140, 6, 6)";
    bottomLeft.style.backgroundColor="rgb(206, 158, 36)";
    bottomRight.style.backgroundColor="rgb(5, 5, 138)";
}
For the winning state or if the player makes a mistake all the colors light up as defined in the function below.
function flashColor(){
    topLeft.style.backgroundColor="lightgreen";
    topRight.style.backgroundColor="tomato";
    bottomLeft.style.backgroundColor="yellow";
    bottomRight.style.backgroundColor="lightskyblue";
}


Now we add an event listener to capture what colors the player clicks and send them to the playerOder array so we can compare them to the computers "order" array
1 is green, 2 is red 3 is yellow and 4 is blue, the if statement checks for what color was click and sends the appropriate number to the playerOrder array.
topLeft.addEventListener('click', (event)=>{
    if(on){
        playerOrder.push(1);
        check();
        one(); the one function flashes green.
        then we check to see if winning conditions are met.
        if(win){
            setTimeout(()=>{
            clearColor();
        }, 500);
      }else{
        setTimeout(()=>{
            clearColor();
        }, 500);
      }
    }
})
topRight.addEventListener('click', (event)=>{
    if(on){
        playerOrder.push(2);
        check();
        two();
        if(win){
            setTimeout(()=>{
            clearColor();
        }, 500);
      }else{
        setTimeout(()=>{
            clearColor();
        }, 500);
      }
    }
})
bottomLeft.addEventListener('click', (event)=>{
    if(on){
        playerOrder.push(3);
        check();
        three();
        if(win){
            setTimeout(()=>{
            clearColor();
        }, 500);
      } else{
        setTimeout(()=>{
            clearColor();
        }, 500);
      }
    }
})
bottomRight.addEventListener('click', (event)=>{
    if(on){
        playerOrder.push(4);
        check();
        four();
        if(win){
            setTimeout(()=>{
            clearColor();
        }, 500);
      } else{
        setTimeout(()=>{
            clearColor();
        }, 500);
      }
    }
})

"Check" function compares the players "playOrder" array to the computers "order" array.
Strict allows no mistakes and restarts the game at level one,
If the strick box is not checked the game continues from the round the player got wrong.

function check(){
 if(playerOrder[playerOrder.length-1] !== order[playerOrder.length-1]) if they are not equal, the player got it wrong which is not good therfore good is set to false.
 good = false;
 if(playerOrder.length== 20 && good){
     winGame(); Did they win?
 }
 if (good==false){
     flashColor();
     turnCounter.innerHTML="Wrong!"; displays wrong
     pauses 800 milliseconds after updaing the round, 
     note: the variables are returned to the starting state if strict is selected.
     setTimeout(()=>{
         turnCounter.innerHTML= turn;
         clearColor();

         if(strict){
             play();
         } else{
             compTurn=true;
             flash=0;
             playerOrder=[];
             good= true;
             intervalId = setInterval(gameTurn,800);
         }
     },800)
     noise = false;
 } 
 
  Checks for a win  and resets the game.
 if (turn== playerOrder.length && good && !win){
     turn++;
     playerOrder =[];
     compTurn = true;
     flash = 0;
     turnCounter.innerHTML= turn;
     intervalId = setInterval(gameTurn,800);
 }
}
Celebrates and displays Win if the player is succesful.
function winGame(){
    flashColor();
    turnCounter.innerHTML="Win!";
    on= false;

}

///console.log('hello')