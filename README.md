## About Letterpress Battle

[Letterpress](https://itunes.apple.com/us/app/letterpress-word-game/id526619424?mt=8) is a great iPhone/iPad game by Loren Brichter.After playing Letterpress happily for several hours, [@batuo](http://twitter.com/baotuo) and [I](http://twitter.com/xu_lele) decided to build a cheater to get all usable words in one game. And the faster one wins.

And after the cheater project, which @baotuo won the first round and I won the second, we decided to continue the competition with game AIs. A tool to monitor and tune the AIs is need by both of us, and that's why ‘Letterpress Battle’ (aka LPB) comes from.

The main purpose of LPB is to let the game AIs build by me and @baotuo to battle together with a common set of APIs. You can fight the AIs too if you like.## Letterpress Battle Client API

Two class is provided in this API.

One is `LPBClient`, which support the following server API
* create - Create a game with a 25-characters board
* list - List all games
* join - Join a game
* move - Take a move
* resign - Resign a game
* show - Show game statistics

The other is `LPBConsole`, which is a interactive to play the game in console.

### Example

	var program = require('commander')
	  , lpb = require('lpb-client');
	
	program
	  .version('0.0.1')
	  .option('-s, --server [url]', 'Base URL of the server')
	  .option('-p, --player [player]', 'Unique player ID')
	  .parse(process.argv);
	
	...
	
	if (program.server && program.player) {
	  c = lpb.LPBConsole(program.server, program.player);
	
	  // logic to handle the move
	  function takeMove() {
	  	...
	  }
	
	  // logic to init the game board
	  function prepareBoard(data) {
	  	...
	  }
	
	  // logic to init the game board
	  function applyMove(data) {
	  	...
	  }

	  c.on('join', prepareBoard);
	
	  c.on('ourmove', takeMove);
	
	  c.on('theirmove', function(move) {
	    applyMove();
	    c.emit('ourmove');
	  });
	
	  c.start();
	} else {
	  program.outputHelp();
	}
