# Trilha JS Developer - Pokedex

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pokédex</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Pokédex</h1>
    <div id="pokemon-container"></div>
    <button id="loadMoreButton">Carregar Mais</button>
    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    text-align: center;
}

#pokemon-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.pokemon-card {
    background-color: white;
    border: 1px solid #ccc;
    border-radius: 8px;
    margin: 10px;
    padding: 20px;
    width: 200px;
}

.type {
    display: inline-block;
    background-color: #ddd;
    color: #fff;
    padding: .25rem .5rem;
    margin: .25rem 0;
    font-size: .625rem;
    border-radius: 1rem;
    text-align: center;
}

.normal { background-color: #a8a878; }
.grass { background-color: #78c850; }
.fire { background-color: #f08030; }
.water { background-color: #6890f0; }
.electric { background-color: #f8d030; }
.ice { background-color: #98d8d8; }
.ground { background-color: #e0c068; }
.flying { background-color: #a890f0; }
.poison { background-color: #a040a0; }
.fighting { background-color: #c03028; }
.psychic { background-color: #f85888; }
.dark { background-color: #705848; }
.rock { background-color: #b8a038; }
.bug { background-color: #a8b820; }
.ghost { background-color: #705898; }
.steel { background-color: #b8b8d0; }
.dragon { background-color: #7038f8; }
.fairy { background-color: #f0b6bc; }
document.addEventListener('DOMContentLoaded', () => {
    const pokemonContainer = document.getElementById('pokemon-container');
    const loadMoreButton = document.getElementById('loadMoreButton');
    let offset = 0;
    const limit = 10;

    function fetchPokemon(offset, limit) {
        fetch(`https://pokeapi.co/api/v2/pokemon?offset=${offset}&limit=${limit}`)
            .then(response => response.json())
            .then(data => {
                data.results.forEach(pokemon => {
                    fetch(pokemon.url)
                        .then(response => response.json())
                        .then(pokemonData => {
                            const pokemonCard = document.createElement('div');
                            pokemonCard.classList.add('pokemon-card');
                            pokemonCard.innerHTML = `
                                <h2>${pokemonData.name}</h2>
                                <img src="${pokemonData.sprites.front_default}" alt="${pokemonData.name}">
                                <p>ID: ${pokemonData.id}</p>
                                <div class="types">
                                    ${pokemonData.types.map(type => `<span class="type ${type.type.name}">${type.type.name}</span>`).join('')}
                                </div>
                            `;
                            pokemonContainer.appendChild(pokemonCard);
                        });
                });
            })
            .catch(error => console.error('Erro ao buscar Pokémon:', error));
    }

    loadMoreButton.addEventListener('click', () => {
        offset += limit;
        fetchPokemon(offset, limit);
    });

    // Carregar os primeiros 10 Pokémon
    fetchPokemon(offset, limit);
});
