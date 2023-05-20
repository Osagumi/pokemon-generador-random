<!DOCTYPE html>
<html>
<head>
  <title>Random Pokémon Generator</title>
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <script>
    $(document).ready(function() {
      var pokemonList = [];

      // Fetch a random Pokémon from the PokeAPI
      function fetchRandomPokemon() {
        var randomId = Math.floor(Math.random() * 898) + 1; // Total number of Pokémon is 898
        var apiUrl = "https://pokeapi.co/api/v2/pokemon/" + randomId;

        $.getJSON(apiUrl, function(data) {
          pokemonList.push(data);
          if (pokemonList.length === 6) {
            displayPokemon();
          }
        });
      }

      // Capitalize each word in a move name
      function capitalizeMoveName(moveName) {
        var words = moveName.split(" ");
        for (var i = 0; i < words.length; i++) {
          words[i] = words[i].charAt(0).toUpperCase() + words[i].slice(1);
        }
        return words.join(" ");
      }

      // Fetch random moves for each Pokémon
      function fetchRandomMoves(pokemon) {
        var moves = [];
        var moveCount = pokemon.moves.length;

        for (var i = 0; i < 4; i++) {
          var randomMoveIndex = Math.floor(Math.random() * moveCount);
          var move = capitalizeMoveName(pokemon.moves[randomMoveIndex].move.name); // Capitalize each word in move name
          moves.push(move);
        }

        return moves;
      }

      // Display the Pokémon and their movesets
      function displayPokemon() {
        var output = "";
        for (var i = 0; i < pokemonList.length; i++) {
          var pokemon = pokemonList[i];
          var moves = fetchRandomMoves(pokemon);

          output += "<div class='pokemon'>";
          output += "<img src='" + pokemon.sprites.front_default + "' alt='" + pokemon.name + "' />";
          output += "<h3>" + pokemon.name + "</h3>";
          output += "<ul>";
          for (var j = 0; j < moves.length; j++) {
            output += "<li>" + "-" + moves[j] + "</li>"; // Add "-" at the beginning
          }
          output += "</ul>";
          output += "</div>";
        }

        $('#pokemon-container').html(output);
      }

      // Generate random Pokémon
      function generatePokemon() {
        pokemonList = []; // Reset the list of Pokémon
        for (var i = 0; i < 6; i++) {
          fetchRandomPokemon();
        }
      }

      // Reroll Pokémon
      $('#reroll-button').click(function() {
        generatePokemon();
      });

      generatePokemon();
    });
  </script>
  <style>
    .pokemon {
      display: inline-block;
      text-align: center;
      margin: 10px;
    }

    ul {
      list-style: none;
      padding: 0;
    }
  </style>
</head>
<body>
  <div id="pokemon-container"></div>
  <button id="reroll-button">Reroll Pokémon</button>
</body>
</html>
