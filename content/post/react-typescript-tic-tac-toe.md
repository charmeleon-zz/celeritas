---
title: "React Typescript Tic Tac Toe"
date: 2018-05-28T19:33:57-05:00
category: TypeScript
tags: react, typescript, javascript
draft: false
---
If you are on the fence about starting your next React project with TypeScript 
but feel that you're lacking an introduction, this one is for you. TypeScript 
syntax should feel familiar to you if you have JavaScript experience and 
becomes intuitive after spending some time on it.

The live demo for the finished version of this project is [here](https://charmeleon.github.io/games-xoxo-ts/), and the code is open source
[on its own Github repo](https://github.com/charmeleon/games-xoxo-ts).

## Motivation
TypeScript is a superset of JavaScript so valid JavaScript is (mostly) valid 
TypeScript. It provides static typing, roughly 
meaning we have to decide whether a variable will be a `number`, `string`, 
`boolean`, etc. when we declare it.

JavaScript is loosely typed so if a 
program has a variable `var counter = 15;`, a new value can be assigned to 
this variable `counter = "potato";`. Clearly `"potato"` is of little use in 
counting, but nothing prevents us from doing this. With TypeScript I can 
declare that `counter` is a `number`, so that attempting to assign anything 
but a `number` will fail: `var counter: number = 15;`

In my experience, TypeScript is very useful in modern web development for non-
trivial web applications. IDEs have a hard time inferring variable
information and giving useful autocomplete options when writing JS, 
refactoring is a mess (IDEs change variables and properties with the same name 
in unrelated files), and many types of bugs that TypeScript helps avoid, like
the "potato" example above, are routine and can be difficult to debug in large 
codebases.

## Prerequisites
In this post I assume you have some JavaScript and React experience. If you 
are learning React and TypeScript at the same time, I recommend focusing on 
React first unless you have previous experience developing web applications.

## What We're Building
This tutorial will focus on building a Tic Tac Toe game using React and
TypeScript. The focus is to provide a gentle introduction to TypeScript with 
React.

The React tutorial steps through building a Tic Tac Toe game as well, 
I will base this project on my pre-existing solution [games-xoxo](https://github.com/charmeleon/games-xoxo/tree/master).

## Tooling
For this project we'll need two npm packages installed globally, `yarn`
and `create-react-app`. Install these via `npm install -g yarn create-react-app`.
I prefer `yarn` over `npm` for package management because it feels faster than 
`npm` and I have less issues with the lockfiles.

## Setup
Now we begin a new project that will use TypeScript and React. The end result 
will be similar to Microsoft's [TypeScript-React-Starter](https://github.com/Microsoft/TypeScript-React-Starter)
which is an excellent guide but assumes familiarity with Redux; this project
does not.

Create the project via the comand line using `create-react-app xoxo-ts --scripts-version=react-scripts-ts`.
Our application is in the `xoxo-ts` directory. This process 
shortcuts a lot of steps in setting up TypeScript and React with an efficient 
bundling pipeline; this is an involved process that I will try to 
address from the ground up in a separate blog post.

Open this project in your favorite editor, and peek inside the `src` 
directory, it should look like this:

    src
    |-- App.css
    |-- App.test.tsx
    |-- App.tsx
    |-- index.css
    |-- index.tsx
    |-- logo.svg
    `-- registerServiceWorker.ts

There are two file extensions of note here: `.ts` and `.tsx`. `.ts` files are
TypeScript files with no JSX. `.tsx` files have JSX syntax in them, and you 
should always import React in `.tsx` files or you will see this error:

    'React' refers to a UMD global, but the current file is a module. Consider adding an import instead.

If you see this error, make sure you have `import * as React from 'react';`
at the top of your `.tsx` files, the error should go away.

Let's add a couple of utility packages: `yarn add classnames lodash`

* `classnames` is a utility for joining classes that is very useful
when working with React
* `lodash` has a collection of useful functions that I use in almost every JavaScript project

Since these packages are not written in TypeScript as of this writing, we
need to add separate dev dependencies to provide TypeScript definitions.
These are typically found under `@types/<package>`; they are open source and
maintained by the TypeScript community: `yarn add -D @types/classnames @types/lodash`.

## Modify tslint Rules
`react-scripts-ts` also configured `tslint` for us. I'm a fan and proponent of
`tslint` but for this project I will make some modifications to keep a light
touch. Open the `tslint.json` file and add the `rules` section as seen below:

    {
        "extends": ["tslint:recommended", "tslint-react", "tslint-config-prettier"],
        "linterOptions": {
            "exclude": [
                "config/**/*.js",
                "node_modules/**/*.ts"
            ]
        },
        "rules": {
            "array-type": [true, "generic"],
            "interface-name":  [true, "never-prefix"],
            "member-access": false,
            "member-ordering": false,
            "no-console": false
        }
    }

Here is the role for each of these rules (I have also linked the name of each
rule to the `tslint` documentation for further explanation):

* [`array-type`](https://palantir.github.io/tslint/rules/array-type/): 
Use `Array<T>` format instead of `T[]` for array types
* [`interface-name`](https://palantir.github.io/tslint/rules/interface-name/): 
Interface names will not have an `I` prefix
* [`member-access`](https://palantir.github.io/tslint/rules/member-access/):
Disable explicit visibility declarations
* [`member-ordering`](https://palantir.github.io/tslint/rules/member-ordering/):
Disable enforcement of member ordering (since visibility declarations are not
explicit anyway)
* [`no-console`](https://palantir.github.io/tslint/rules/no-console/):
Allow the use of `console.*` methods

## Start Editing
Let's begin! First we'll add a `GameControls.tsx` file inside `src`. The 
contents should look like this:

    import * as React from 'react';

    export const GameControls = (props) => (
        <div className="controls">
            <button key="reset" onClick={props.reset}>Reset</button>
            <button key="sound" onClick={props.toggleSound}>Sound</button>
        </div>
    );

This is a functional React component, there is no type information yet. Next, 
edit `App.tsx` to import `GameControls`, replace its entire contents with this:

    import * as React from 'react';
    import './App.css';
    import { GameControls } from './GameControls';

    class App extends React.Component {
        public render() {
            return (
                <div>TODO</div>
            );
        }
    }

    export default App;


After saving you will see this error: `'GameControls' is declared but its value is never read.`
TypeScript can be very strict. It's tempting (and easy) to configure it to
be more forgiving but I suggest getting used to its strict nature.

We'll ignore the fact that `GameControls` is unused for now and add our 
first TypeScript definitions. I will cover `GameControls` in detail; the other 
components will move at a faster pace.

## Typing The First Component

If you look at `GameControls`, the `props` argument has a particular shape:
it has a `reset` and a `toggleSound` property, and both of those are being
passed to an `onClick` event on a button, so they are functions. In 
TypeScript we express the overall shape for objects as an `interface`. 
Interfaces are not functions or classes or objects. They're a collection of 
types, and their job is to describe the types a developer should embed into
an object. This is an interface that recognizes that `reset` and `onClick` are functions:

    export interface GameControlsProps {
        reset: Function;
        toggleSound: Function;
    }
    // `props` argument must match the shape of the `GameControlProps` interface
    export const GameControls = (props: GameControlsProps) => {
        ...
    }

This is a good start, but it doesn't go far enough. You may see a nasty error 
that says `Type '{ children: string; key: string; onClick: Function; }' is not assignable to type 'DetailedHTMLProps<ButtonHTMLAttributes<HTMLButtonElement>, HTMLButtonElement>'.`
It turns out, the `onClick` event of a `button` element has a particular 
shape (interface) as well. Once we fix our `GameControlProps` interface, 
this error goes away:

    export interface GameControlsProps {
        reset: React.MouseEventHandler<HTMLElement>;
        toggleSound: React.MouseEventHandler<HTMLElement>;
    }

A note about the 
`<HTMLElement>` syntax. `React.MouseEventHandler` is what's known as a
[generic](https://www.typescriptlang.org/docs/handbook/generics.html) 
interface. A generic interface a reusable interface where the implementation
will usally constrain some implementation detail to that specific type.
If we had a `sort` generic interface, sorting strings, numbers or objects is different internally, but our interface only has to account for the types:
passing a list of strings returns a sorted list of strings, etc.

The `React.MouseEventHandler` interface is used for several types of 
elements that the user can interact with via the mouse - `HTMLElement` is 
the catch-all for any HTML element, but there's also `HTMLAnchorElement` 
for the `<a>` tag, `HTMLButtonElement` for a `<button>`, `HTMLDivElement` 
for a `<div>`, etc.

`React.MouseEventHandler<HTMLElement>` actually describes a function! It's 
a function most JavaScript developers are familiar with, an event
callback (aka event handler). It has this shape: `function(event) {...}`
in JavaScript. In TypeScript we get a souped up version that looks like this: `function(event: MouseEvent<HTMLElement>): void {...}`. Note that the `void`
return type means this function does not return a value, and trying to return
a value will get you in trouble with the TypeScript compiler.

Here is the `MouseEvent` interface definition:

    interface MouseEvent<T> extends SyntheticEvent<T> {
        altKey: boolean;
        button: number;
        buttons: number;
        clientX: number;
        clientY: number;
        ctrlKey: boolean;
        getModifierState(key: string): boolean;
        metaKey: boolean;
        nativeEvent: NativeMouseEvent;
        pageX: number;
        pageY: number;
        relatedTarget: EventTarget;
        screenX: number;
        screenY: number;
        shiftKey: boolean;
    }

Notice again the generic: `interface MouseEvent<T> extends 
SyntheticEvent<T>`. Another feature of interfaces is that they can be
extended. By extending, this interface will have all properties of 
`SyntheticEvent` as well as everything in `MouseEvent`. Here is a snapshot
of `SyntheticEvent`:

    interface SyntheticEvent<T> {
        bubbles: boolean;
        currentTarget: EventTarget & T;
        cancelable: boolean;
        defaultPrevented: boolean;
        eventPhase: number;
        isTrusted: boolean;
        nativeEvent: Event;
        preventDefault(): void;
        isDefaultPrevented(): boolean;
        ...
    }

Here we see how a function can define its return value:

*  `isDefaultPrevented (): boolean` the function returns a
`boolean`
* `preventDefault(): void` the function does not return a value

The most interesting property here is `currentTarget`, whose type is set to
`EventTarget & T`. This is known as an [Intersection Type](https://www.typescriptlang.org/docs/handbook/advanced-types.html),
an advanced type that combines multiple types into one; in this case
`currentTarget` has all of the properties of `EventTarget` as well as
all the properties of the generic (in our case, `HTMLElement`).

That was a deep dive for an introductory TypeScript lesson, but there are 
many types built-in to React and other libraries, and using them
properly takes a lot of practice and learning to read TypeScript definitions 
so I'm hoping to help jump-start your progress.

Now the final touch is to properly express that our `GameControls` component
is a functional React component that takes props in the shape of the 
`GameControlProps` interface that we have defined; this is what the final 
revision looks like:

    export const GameControls: React.SFC<GameControlsProps> = props => (
        <div className="controls">
            <button key="reset" onClick={props.reset}>Reset</button>
            <button key="sound" onClick={props.toggleSound}>Sound</button>
        </div>
    );

`React.SFC` is the type to use for stateless functional components. The 
TypeScript compiler will recognize the `props` to be whatever type you specify 
in the generic (in our case, `GameControlProps`).

## Other Components

Next we'll add a `Square` UI component representing a square in a tic tac toe
board. Here's what the untyped `Square.tsx` will look like:

    import * as React from 'react';
    import * as classNames from 'classnames';

    export const Square = props => (
        <button className={classNames('square', { winner: props.isWinner })} onClick={() => props.onClick(props.index)}>
            {props.value}
        </button>
    );

The `props` for a `Square` have a particular shape:

* an `isWinner` prop telling us if this Square is part of a winning line
* an `index` prop identifying this `Square` on the game board
* an `onClick` event handler prop
* a `value` prop

The `value` is interesting here, it should be any type that is valid as a 
React child. There is a type for exactly this, it's `React.ReactNode`. 
Changing this from a functional component to a class component that has no state (in order to avoid the anonymous function `() => props.onClick
(props.index)`), the typed version of `Square` is this:

    import * as classNames from 'classnames';
    import * as React from 'react';

    interface SquareProps {
        isWinner?: boolean;
        index: number;
        onClick(index: number): void;
        value: React.ReactNode;
    }

    export class Square extends React.Component<SquareProps> {

        onClick = (): void => {
            this.props.onClick(this.props.index);
        }

        render() {
            return (
                <button className={classNames('square', { winner: this.props.isWinner })} onClick={this.onClick}>
                    {this.props.value}
                </button>
            );
        }
    }

Notice that the `isWinner` prop is an optional prop - it doesn't need to 
specified every time. If it isn't set we can assume that this `Square` is not
a winner and give it only a `square` class, no `winner` class (this is 
taken care with our use of the `classnames` package).

Next, add a `Board.tsx` file with a `Board` UI component representing the
3x3 grid for a tic tac toe game:

    import * as React from 'react';
    import { Square } from './Square';

    class Board extends React.Component {
        renderSquare(i) {
            const isWinner = this.props.winnerLine && this.props.winnerLine.indexOf(i) > -1;

            return (
                <Square
                    value={this.props.squares[i]}
                    index={i}
                    onClick={this.props.onClick}
                    isWinner={isWinner}
                />
            )
        }
        renderRow(j) {
            return (
                <div className="board-row">
                    {this.renderSquare(j)}
                    {this.renderSquare(j + 1)}
                    {this.renderSquare(j + 2)}
                </div>
            );
        }
        render() {
            return (
                <div>
                    {this.renderRow(0)}
                    {this.renderRow(3)}
                    {this.renderRow(6)}
                </div>
            );
        }
    }

    export default Board

The component takes three props: `winnerLine`, `squares`, and `onClick`.

This `onClick` is different from previous ones, it will be invoked with the
index of the `Square` that is clicked. It is therefore not a match for 
the `React.MouseEventHandler`interface, but a custom function that accepts
a number as a parameter. The resulting code is this:

    import * as React from 'react';
    import { Square } from './Square';

    export type SquareState = 'X' | 'O' | null;

    export interface BoardProps {
        onClick(index: number): void;
        squares: Array<SquareState>;
        winnerLine?: Array<number>;
    }

    class Board extends React.Component<BoardProps> {

        renderSquare(i: number): React.ReactNode {
            const isWinner = this.props.winnerLine && this.props.winnerLine.indexOf(i) > -1;

            return (
                <Square
                    value={this.props.squares[i]}
                    index={i}
                    onClick={this.props.onClick}
                    isWinner={isWinner}
                />
            )
        }
        renderRow(j: number): React.ReactNode {
            return (
                <div className="board-row">
                    {this.renderSquare(j)}
                    {this.renderSquare(j + 1)}
                    {this.renderSquare(j + 2)}
                </div>
            );
        }
        render() {
            return (
                <div>
                    {this.renderRow(0)}
                    {this.renderRow(3)}
                    {this.renderRow(6)}
                </div>
            );
        }
    }

    export default Board;

Here, `renderSquare` and `renderNode` specify that they will return a 
`React.ReactNode`, but typically the `render` method's return value is not 
specified.

This file defines a custom type, `SquareState` which restricts the
`value` prop for the squares to one of `X`, `O` or `null`.

Now add a file `Functions.ts` that will be the home for the logic to 
check the win conditions. There are two functions, `calculateWinner`
to determine whether X or O (or neither) has won, and `getWinnerLine` that 
will return the indices of each square in a winning line, or null.

Only function arguments and return values have their types specified in this 
file:

    import { SquareState } from './Board';

    export function calculateWinner(squares: Array<SquareState>): SquareState {
        const line = getWinnerLine(squares);

        if (line) {
            return squares[line[0]];
        }

        return null;
    }

    export function getWinnerLine(squares: Array<SquareState>): Array<number> | undefined {
        const lines = [
            // horizontal
            [0, 1, 2],
            [3, 4, 5],
            [6, 7, 8],
            // vertical
            [0, 3, 6],
            [1, 4, 7],
            [2, 5, 8],
            // across
            [0, 4, 8],
            [2, 4, 6]
        ];

        for (const line of lines) {
            const [a, b, c] = line;
            if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
                return line;
            }
        }

        return undefined;
    }

In `getWinnerLine`, the `lines` variable can be specified as
a two-dimensional array of numbers: `const lines: Array<Array<number>> = ...`
but the TypeScript compiler has enough information to infer this so 
there is no need. When the type is ambiguous the compiler will ask you
specify it.

I find it useful to ensure that the inputs (function arguments, 
props) and outputs (return values) are typed to protect against bugs 
arising from future changes to the codebase and work with the compiler on 
types that it cannot infer. I don't actively work to having _every_ 
variable in my code typed (but I do work to fix every TypeScript error).

Before the next section, download the [sound file](https://github.com/charmeleon/games-xoxo/blob/master/public/assets/sounds/electro-bass.wav)
(or comment it out from the code below).

Now, for the last component to tie all this together, `App.tsx` should look 
like this:

    import * as React from 'react';
    import { isNull, isNullOrUndefined, isUndefined } from 'util';
    import './App.css';
    import Board, { SquareState } from './Board';
    import { calculateWinner, getWinnerLine } from './Functions';
    import { GameControls } from './GameControls';

    type BoardState = Array<SquareState>;

    export interface AppState {
        history: Array<BoardState>,
        playSound: boolean;
        stepNumber: number;
        winner: SquareState;
        winnerLine?: Array<number>;
        xIsNext: boolean;
    }

    class App extends React.Component<{}, AppState> {
        constructor(props: {}) {
            super(props);
            this.reset();
        }

        reset = () => {
            const playSoundStored = localStorage.getItem('playSound');
            const playSound = isNull(playSoundStored) ? true : JSON.parse(playSoundStored);
            const history = [
                Array(9).fill(null)
            ];

            const state = {
                history,
                playSound,
                stepNumber: 0,
                winner: null,
                winnerLine: undefined,
                xIsNext: true,
            };

            if (isUndefined(this.state)) {
                this.state = state;
            } else {
                this.setState(state);
            }
        }

        handleBoardClick = (i: number) => {
            const history = this.state.history;
            // The board's current state is the latest entry in this.state.history
            const boardState = history.slice(-1).pop() as BoardState;
            // Ignore click if the square already has a value, or if the game is over
            if (!isNullOrUndefined(boardState[i]) || this.state.winner) {
                return;
            }
            // Update this square's value
            boardState[i] = this.state.xIsNext ? 'X' : 'O';
            const winner = calculateWinner(boardState);
            const winnerLine = getWinnerLine(boardState);

            this.setState({
                history: history.concat([boardState]),
                stepNumber: this.state.stepNumber + 1,
                winner,
                winnerLine,
                xIsNext: !this.state.xIsNext,
            })
        }

        jumpTo(step: number) {
            this.setState({
                stepNumber: step,
                xIsNext: (step % 2) ? false : true
            })
        }

        toggleSound = () => {
            const playSound = !this.state.playSound;

            localStorage.setItem('playSound', playSound.toString());
            this.setState({
                playSound
            });
        }

        getSound(): React.ReactNode {
            if (this.state.playSound) {
                return (
                    <audio id="background-sound" autoPlay={true} loop={true} src="assets/sounds/electro-bass.wav"/>
                );
            }
            return null;
        }

        getStatusText():  string {
            if (this.state.winner) {
                return 'Winner: ' + this.state.winner;
            }
            else if (this.state.stepNumber >= 9) {
                return "It's a tie!";
            }
            return 'Current turn: ' + (this.state.xIsNext ? 'X' : 'O');
        }

        render() {
            const history = this.state.history;
            const current = history[this.state.stepNumber];
            const status = this.getStatusText();

            return (
                <div className="App">
                    <div className="game-info">
                        <div className="text-center">{status}</div>
                    </div>
                    <div className="game-board">
                        <Board
                            squares={current}
                            winnerLine={this.state.winnerLine}
                            onClick={this.handleBoardClick}
                        />
                    </div>
                    <GameControls
                        toggleSound={this.toggleSound}
                        reset={this.reset}
                    />
                    {this.getSound()}
                </div>
            );
        }
    }

    export default App;

This final component has `state` but no `props` (previously, the `Board` 
component had `props` but no `state`). It reuses the `SquareState` type from
the `Board` component (in a larger application, you might dedicate a directory
in your architecture to these reusable types). There's a good chunk of logic
dedicated to handling a click on the board, but none of it is TypeScript 
intensive.

Finally, these are the contents of my `App.css` to get this game looking 
spiffy:

    :root {
        --square-size:84px;
        --square-font-size:72px
    }
    .App {
        padding-top:2em;
        text-align:center
    }
    .App-logo {
        -webkit-animation:App-logo-spin infinite 20s linear;
        animation:App-logo-spin infinite 20s linear;
        height:80px
    }
    .App-header {
        height:90px;
        padding:20px;
        color:#fff
    }
    .App-intro {
        font-size:large
    }
    @-webkit-keyframes App-logo-spin {
        0% {
            -webkit-transform:rotate(0deg);
            transform:rotate(0deg)
        }
        to {
            -webkit-transform:rotate(1turn);
            transform:rotate(1turn)
        }
    }
    @keyframes App-logo-spin {
        0% {
            -webkit-transform:rotate(0deg);
            transform:rotate(0deg)
        }
        to {
            -webkit-transform:rotate(1turn);
            transform:rotate(1turn)
        }
    }
    body {
        font:18px Helvetica,Arial,sans-serif;
        margin:30px
    }
    ol,ul {
        padding-left:30px
    }
    .status {
        margin-bottom:10px
    }
    .square {
        background:#fff;
        border:1.7px solid #000;
        float:left;
        font-size:var(--square-font-size);
        font-weight:700;
        line-height:var(--square-size);
        height:var(--square-size);
        margin-right:-1px;
        margin-top:-1px;
        padding:0;
        text-align:center;
        vertical-align:middle;
        width:var(--square-size);
        margin-bottom:0;
        border-radius:0
    }
    .board-row:first-child > .square {
        border-top:none
    }
    .board-row:last-child > .square {
        border-bottom:none
    }
    .board-row>.square:first-child {
        border-left:none
    }
    .board-row>.square:last-child {
        border-right:none
    }
    .kbd-navigation .square:focus {
        background:#ddd
    }
    .game {
        -webkit-box-orient:horizontal;
        -webkit-box-direction:normal;
        -ms-flex-direction:row;
        flex-direction:row
    }
    .board-row,.game {
        display:-webkit-box;
        display:-ms-flexbox;
        display:flex
    }
    .board-row {
        -webkit-box-pack:center;
        -ms-flex-pack:center;
        justify-content:center
    }
    .board-row:after {
        clear:both;
        content:"";
        display:table
    }
    .game-info {
        margin-bottom:1em
    }
    .controls {
        padding:2em
    }
    .winner {
        -webkit-animation:blinker 1s linear infinite;
        animation:blinker 1s linear infinite;
        color:green!important
    }
    @-webkit-keyframes blinker {
        50% {
            opacity:0
        }
    }
    @keyframes blinker {
        50% {
            opacity:0
        }
    }

Now you have run a true small TypeScript+React project!