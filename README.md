Bowling Score Tracker 🎳
========================

[Dependencies](#dependencies) | [Getting Started](#getting-started) | [Running Tests](#runnning-tests) | [Objectives](#objectives) | [Design](#design) | [Usage](#usage) | [User Stories](#user-stories)

[![Build Status](https://travis-ci.com/AJ8GH/bowling-challenge.svg?branch=master)](https://travis-ci.com/AJ8GH/bowling-challenge) [![Coverage Status](https://coveralls.io/repos/github/AJ8GH/bowling-challenge/badge.svg?branch=master)](https://coveralls.io/github/AJ8GH/bowling-challenge?branch=master) [![Maintainability](https://api.codeclimate.com/v1/badges/a4fa6060a3a3e9fe32ef/maintainability)](https://codeclimate.com/github/AJ8GH/bowling-challenge/maintainability) ![Style](https://img.shields.io/badge/code_style-standard-brightgreen.svg) [![BCH compliance](https://bettercodehub.com/edge/badge/AJ8GH/bowling-challenge?branch=master)](https://bettercodehub.com/)

Bowling score tracker written in JavaScript

**Use the deployed app here:**

[Bowling Score Tracker](https://bowling-score-tracker.surge.sh/)
--------------------------------------------------------

![index](public/images/index.png)

## Dependencies
- `"coveralls": "^3.1.0"`
- `"eslint": "^7.21.0"`
- `"eslint-config-airbnb-base": "^14.2.1"`
- `"eslint-plugin-import": "^2.22.1"`
- `"jasmine": "^3.6.4"`
- `"karma": "^6.2.0"`
- `"karma-chrome-launcher": "^3.1.0"`
- `"karma-cli": "^2.0.0"`
- `"karma-coverage": "^2.0.3"`
- `"karma-coveralls": "^2.1.0"`
- `"karma-jasmine": "^4.0.1"`
- `"nyc": "^15.1.0"`

## Getting Started

Start by cloning this repository

```shell
git clone git@github.com:AJ8GH/bowling-challenge.git
```

Ensure you have Node installed, by running `node -v`. If not you can download it [here](https://nodejs.org/en/download/).

Navigate to the root of the project and install the dependencies.

```shell
cd bowling-score-tracker
npm install
```

## Runnning tests:

To run tests from the terminal, run `npm test`

To run tests in the browser, open `public/js/spec/SpecRunner.html` in the browser, which also gives an overview of the public interfaces and functionality of the classes and the app as a whole.

## Objectives

The purpose of this project was to build a score calculator for 10 pin bowling. Bowling is a deceptively complex game and the goal here was to build a working app with high code quality, using test driven development. Once the game logic was complete I used jQuery, HTML and CSS to create a responsive UI and deployed the app.

### Testing:
- I used Jasmine to write automated unit and feature tests which can be run in the browser. The code base was written using TDD, with a red-green-refactor approach.
- The feature specs focus on running through an entire game, to ensure the program functions as expected. The unit specs test individual functions in isolation.
- I chose to mock the Frame class in the game class tests, using dependency injection, to ensure the classes were tested in isolation:

```js
// dependency injection in game.js
class Game {
  constructor (frameClass = Frame) {
  // ...

// mocking the implementation of Frame in gameSpec.js using Jasime spies
  beforeEach(() => {
    frameClass = jasmine.createSpy('frameClass');
    game = new Game(frameClass);
    // ...
```

- I used Karma and ChromeHeadless to enable runnings tests from the terminal. This then enabled me to Implement CI using Travis.
- Using NYC and Coveralls I then set up automated test coverage reports for the codebase.
- Note - NYC currently fails to report coverage when running tests locally, however the stats are accurately sent to coveralls, which shows > 97% test coverage.

### Edge cases:
- Guard conditions are implemented to prevent invalid inputs. Errors will be thrown when non-numbers or 'empty' rolls are entered, as well as if the roll is greater than the number of reamining pins for the frame.
- An error is also thrown when attempting to input a roll when the game is over.

### Code Quality:
- My focus was around building encapsulated classes and single responsibility functions, to ensure the code is as readable and maintainable as possible.
- For linting I used ESLint to enforce the standard JS style
- I used CodeClimate and Better Code Hub to further check the repository for any code smells or technical debt. Both are reporting maximum code quality scores.

### Workflow:
- My git workflow consists of making small and consistent commits at every green and refactor stage.
- Applying agile processes in translating specifications into user stories and then test driving and implementing those features

### Documentation:
- My aim is to create comprehensive and easy to use documentation, to ensure other developers could use and contribute to the project with ease.

### Design:
- Private functions are prefixed with underscores
- Predicate functions which return boolean values begin with the word 'is', e.g. `game.isOver()`
- **Game class**: responsible for tracking the frames and the progress of the game
- **Frame class**: responsible for tracking its rolls, score and bonuses
- **Interface**: Built using js and jQuery, responsible for updating the view after each roll input

## Usage

Public Interfaces:

**Game**
- `#addRoll()` -Takes integer argument between 0 and 10. Throws error if the game is over or if an invalid input is entered
- `#totalScore()` - Returns the sum of the scores of all frames so far
- `#calculateScores()` - Returns an array of the total gamescore so far for each frame. e.g. after 3 strikes, it would return `[30, 50, 60]`
- `isOver()` - Returns true or false if game is over or not over

**Frame**
The Game object interacts with the Frames, the user does not interact with Frame objects directly
- `#addRoll()` - Takes integer argument, adds roll to `this.rolls` array
- `#score()` - Returns the total score of the frame instance
- `#makeFinal()` - sets `this.isFinal` to true, to adjust logic for the final frame
- `#addBonus()` - Adds bonus score if needed (frames are aware of how many bonuses they need)
- `#isOver()` - returns true if frame is over, false if it's not

![perfect-game](public/images/perfect-game.gif)

***A perfect Game***

![gutter-game](public/images/gutter-game.gif)

***Gutter game***

### Sequence Diagrams

#### Spare Bonus

![spare-bonus](public/images/spare-bonus.png)

#### Strike Bonus

![strike-bonus](public/images/strike-bonus.png)

### User Stories
```
As a bowler,
So that I can track my score while I play,
I want to be able to record a score from 1 roll.

As a bowler,
So that I can track my total score easily throughout a game,
I want my scores to automatically accumlate as I enter them.

As a bowler,
So that my score is accurate and my experience is enjoyable,
I want my bonuses to be added to frames automatically.

As a bowler,
So that I can get extra points,
I want the final frame to allow a bonus roll if I get a spare.

As a bowler,
So that I can bowl a perfect game,
I want the final frame to allow 2 bonus rolls if I get a strike.

As a bowler,
So that my game flows and I can focus on the bowling,
I want the game to automatically register when it is the final frame.
```
