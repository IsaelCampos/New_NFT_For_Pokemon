# Criação de uma NFT de Pokémon

Vamos utilizar o Solidity para desenvolver o nosso token NFT no padrão ERC-721, simulando um jogo de batalhas entre Pokémons.

## Tecnologias utilizadas

* Solidity - `Linguagem`;
* Ganache - `Ambiente Blockchain`;
* Remix IDE - `Códificação`;
* Metamask - `Gerenciador de carteiras`;
* IPFS - `Frameworks`;

## Etapas do desafio

* Implementar o token ERC-721;
* Publicar na blockchain;
* Realizar "batalhas" com os Pokémons;
* Transferir NFT entre contas.

---
Explorando os métodos propostos pelo:

`import "@openzeppelin/contracts/token/ERC721/ERC721.sol";`

Obs:. As contas e carteiras utilizadas e também a rede teste fora retiradas e vinculadas do `Ganache`.
---
## Implementação da `batalha pokémon`.

```solidity
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
// Importação do contrato ERC721 do OpenZeppelin

contract PokeDIO is ERC721 {
    // Declaração do contrato PokeDIO, herdando do contrato ERC721

    struct Pokemon {
        string name;
        uint level;
        string img;
    }
    // Declaração de uma estrutura para representar um Pokémon

    Pokemon[] public pokemons;
    // Declaração de uma matriz pública de Pokémons

    address public gameOwner;
    // Declaração de uma variável pública para armazenar o endereço do proprietário do jogo

    constructor() ERC721("PokeDio", "PKD") {
        // Função de construtor do contrato

        gameOwner = msg.sender;
        // Define o endereço do criador do contrato como proprietário do jogo
    }

    modifier onlyOwnerOf(uint _monsterId) {
        // Modificador para permitir apenas que o dono do Pokémon execute determinadas funções

        require(ownerOf(_monsterId) == msg.sender, "Apenas o dono pode batalhar com este Pokemon");
        // Verifica se o remetente da transação é o dono do Pokémon especificado
        _;
    }

    function battle(uint _attackingPokemon, uint _defendingPokemon) public onlyOwnerOf(_attackingPokemon) {
        // Função para simular uma batalha entre dois Pokémons

        Pokemon storage attacker = pokemons[_attackingPokemon];
        // Obtém uma referência ao Pokémon atacante
        Pokemon storage defender = pokemons[_defendingPokemon];
        // Obtém uma referência ao Pokémon defensor

        if (attacker.level >= defender.level) {
            // Verifica se o nível do atacante é maior ou igual ao do defensor
            attacker.level += 2;
            defender.level += 1;
            // Incrementa o nível do atacante em 2 e do defensor em 1
        } else {
            attacker.level += 1;
            defender.level += 2;
            // Incrementa o nível do atacante em 1 e do defensor em 2
        }
    }

    function createNewPokemon(string memory _name, address _to, string memory _img) public {
        // Função para criar um novo Pokémon

        require(msg.sender == gameOwner, "Apenas o dono do jogo pode criar novos Pokemons");
        // Verifica se o remetente da transação é o proprietário do jogo
        uint id = pokemons.length;
        // Obtém o próximo ID de Pokémon disponível
        pokemons.push(Pokemon(_name, 1, _img));
        // Adiciona um novo Pokémon à matriz de Pokémons
        _safeMint(_to, id);
        // Cria uma nova instância do token ERC721 e atribui ao endereço especificado
    }
}

```
---
A carteira lider ou dono, poderá implementar novos pokémons e enviar as `NFT's` para outras carteiras.

Vamos importar a imagem para o `IPFS` o link gerado atribuiremos a `string img` declarada em `struct Pokemon{}`.

```solidity
    struct Pokemon {
        string name;
        uint level;
        string img;
    }
    // Declaração de uma estrutura para representar um Pokémon
```
