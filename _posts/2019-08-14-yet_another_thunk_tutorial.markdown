---
layout: post
title:      "Yet Another Thunk Tutorial"
date:       2019-08-14 19:31:14 +0000
permalink:  yet_another_thunk_tutorial
---


When doing a google image search for "thunk", the first image that popped up for me was:

![thunk emoji](https://statici.behindthevoiceactors.com/behindthevoiceactors/_img/chars/thunk-the-croods-3.jpg)

Which really sums up my process in trying to understand this tool.

Thunks are pretty powerful, and deserve a lot more than our half of a readme. I ended up doing some reading on the topic, and hopefully I can synthasize some useful information for y'all. At the very least, writing it all down here will help cement it in my mind. I gain a lot from [this article](https://medium.com/@stowball/a-dummys-guide-to-redux-and-thunk-in-react-d8904a7005d3) from medium.com and Dave Ceddia's "[What the heck is a thunk?](https://daveceddia.com/what-is-a-thunk/)".

At a very basic level, Thunk lets us do one thing differently: Our action creators can return a *funtion* instead of an object. While I am sure there are *many* uses for a thunk I haven't discovered, I am going to cover three that were a huge benefit to my React/Redux project: getState(), multiple dispatches, and async fetch calls.

## Setup
(this is assuming you already have Redux up and running)

`npm install redux-thunk`

Install thunk, then connect it to your store via redux's `applyMiddleware` like so:

```
// index.js
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers/rootReducer';   // or wherever you stored your rootReducer

const store = createStore(rootReducer, applyMiddleware(thunk));
```

That's it! Now our action creators can look something like this:
```
// actions/GameActions.js

export function drawCard() {
  return (dispatch, getState) => {
    const wholeState = getState();
    if (wholeState.deck.length > 0) {
      dispatch({
        type: "DRAW_CARD",
        newBird: wholeState.deck[0]
      });
    }
  };
}
```

When running our action, we can return a function that takes two very helpful arguments: `dispatch` and `getState`. Let's dive in!

## getState

When working with several reducers, your reducers only have access to their own slice of state. If you store a player's hand of cards in state.player.hand and the deck of cards in state.game.deck, the playerReducer won't be able to see the deck portion of the state, and the gameReducer can't see the player's hand. Enter `getState`. Let's copy the example from above:

```
// actions/GameActions.js

export function drawCard() {
  return (dispatch, getState) => {
    const wholeState = getState();
    dispatch({
      type: "DRAW_CARD",
      newCard: wholeState.game.deck[0]
    });
  };
}
```

`const wholeState = getState()` let's us access the entire state, so I can then call the DRAW_CARD dispatch and pass in the card I want added to the player's hand, and removed from the deck. Neato!

## Multiple Dispatches
The function also received `dispatch` as an argument, so you can call several dispatches all in a row.

```
export function doSoMuch() {
  return dispatch () => {
    dispatch({type: "RESET_STATE});
    dispatch({type: "NEW_GAME"});
    dispatch({type: "FIRST_TURN"});
  }
}
```

This can be accomplished in other ways, such as calling each of these in your controller, or setting up in your reducer that "RESET_GAME" accomplishes all of these, but this is another way you can organize your code.

## Fetch and async calls
With thunk, your actions can return a `Promise`, which I interpret in my head as the computer saying "Hey, js client, I *promise* you I will keep working on this thing (such as a fetch call), but you go on with your code and I'll get back to you soon." Once the promise resolves, you can do follow up actions with `.then()`. For example:

```
export function getGames() {
  return dispatch => {
    dispatch({type:"LOADING_SCORES"})
    fetch('http://localhost:3001/api/high_scores')
      .then(resp => resp.json())
      .then(scores => dispatch({type:"SET_SCORES", payload: scores}))
  }
}
```

In the sub-function, it sends a dispatch to tell my app that I am loading past scores, then it hits a fetch call, which returns a `Promise` object. The client continues about it's day, and whenever the fetch call resolves, it does some things with the response with the `.then()` chain, converting the reponse to a json and calling a dispatch with that data (finishing the loading process).

This can even be used in conjunction with your components! I wanted to save a game to my server, and once the game is saved, redirect to the '/game_over' page. With react-router v4, this wasn't an easy thing to accomplish in the action itself, but knowing my action returned a `Promise`, which can be chained with `.then()`, I did the following:

```
// actions/GameActions.js
export function gameOver(){
  return getState => {
    // gather the game data into a json
    const gameState = getState();
    const gameData = {
      player: gameState.player.playerName,
      score: gameState.player.score,
      season: gameState.game.season.name
    }

    // post the data to the server, which returns a promise
    return fetch('http://localhost:3001/api/games', {
      headers: {
        'Content-Type': 'application/json'
      },
      method: 'POST',
      body: JSON.stringify(gameData)
    })
  }
}

// containers/Game.js

...
class Game extends React.Component {
  componentDidUpdate(){
    if (this.props.game.finished){
      this.props.gameOver()     // calls the gameOver action, which returns a promise, and when that resolves:
        .then(() => this.props.history.push('/game_over'));     // redirect to the game over page!
    }
  }
...
```

## We have thunk it out

Again, I am sure there are a lot more, cooler things that thunk can do, but maybe these will help you out in your thunk understanding! And hopefully you've gone from:

![thunk emoji](https://statici.behindthevoiceactors.com/behindthevoiceactors/_img/chars/thunk-the-croods-3.jpg)

to:

![thunk happy](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRao0-cY8sj8B0FyqEQ-Jey1sNDrwGl0CAIXBFG3G9xJVSP-YXx)
