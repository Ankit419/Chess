<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Free Chess Analyzer</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/chessboard.js/1.0.0/chessboard-1.0.0.min.css">
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #2E2E2E;
            color: #FFFFFF;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
        }

        #board {
            width: 400px;
            margin-bottom: 20px;
        }

        textarea, input {
            width: 80%;
            max-width: 400px;
            padding: 10px;
            margin-bottom: 10px;
            background-color: #1C1C1C;
            color: white;
            border: 1px solid #444;
            border-radius: 5px;
        }

        button {
            background-color: #1DB954;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            margin: 5px;
            cursor: pointer;
        }

        button:hover {
            background-color: #1AAE47;
        }

        .player-info {
            display: flex;
            justify-content: space-between;
            width: 80%;
            max-width: 400px;
            margin-bottom: 10px;
        }

        .player {
            font-size: 1.2em;
        }

        .controls {
            display: flex;
            justify-content: space-evenly;
            width: 80%;
            max-width: 400px;
        }
    </style>
</head>
<body>

    <div class="player-info">
        <div id="black-player" class="player">Black Player (?)</div>
        <div id="white-player" class="player">White Player (?)</div>
    </div>

    <div id="board"></div>

    <textarea id="pgn-input" placeholder="Enter PGN..."></textarea>
    <button onclick="loadPGN()">Analyse</button>

    <div class="controls">
        <button onclick="firstMove()">&#x23EE;</button>
        <button onclick="prevMove()">&#x25C0;</button>
        <button onclick="nextMove()">&#x25B6;</button>
        <button onclick="lastMove()">&#x23ED;</button>
    </div>

    <footer>
        <p>A website by wintrcat <a href="#">Privacy Policy</a></p>
    </footer>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/chess.js/0.10.3/chess.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/chessboard.js/1.0.0/chessboard-1.0.0.min.js"></script>
    <script>
        var board = Chessboard('board', {
            draggable: true,
            position: 'start'
        });

        var game = new Chess();
        var moves = [];
        var currentMove = 0;

        function loadPGN() {
            var pgn = document.getElementById('pgn-input').value;
            game.load_pgn(pgn);
            moves = game.history({ verbose: true });
            currentMove = 0;
            updateBoard();
            updatePlayerNames();
        }

        function updateBoard() {
            game.reset();
            for (var i = 0; i < currentMove; i++) {
                game.move(moves[i].san);
            }
            board.position(game.fen());
        }

        function updatePlayerNames() {
            document.getElementById('black-player').textContent = game.header().Black || "Black Player (?)";
            document.getElementById('white-player').textContent = game.header().White || "White Player (?)";
        }

        function nextMove() {
            if (currentMove < moves.length) {
                currentMove++;
                updateBoard();
            }
        }

        function prevMove() {
            if (currentMove > 0) {
                currentMove--;
                updateBoard();
            }
        }

        function firstMove() {
            currentMove = 0;
            updateBoard();
        }

        function lastMove() {
            currentMove = moves.length;
            updateBoard();
        }
    </script>

</body>
</html>
