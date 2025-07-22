---
Criado por: "[[Contato. Gabriel Wessling]]"
Trabalhando na issue: "[[Contato. Gabriel Wessling]]"
---
## Tarefa
#### Checklist:
- [x] salvamento e carregamento deletam tudo nessa lista.
- [x] criar garantia de que nenhuma informação/data possa existir fora da border enquanto editando um mapa.
- [x] Atualizar TileManipulatorEditorUI para respeitar essas bordas
- [x] Extra: Trazer a possibilidade de segurar o botão do mouse no modo single do add e remove pra pintar por onde o mouse passar
---
## Dev
#### Notas:
- Fiz uma alteração no `Assets\Scripts\Structural\Map\MapDataSerializer.cs` pra que quando ambas as funções (save e load) chegam na parte de verificar cada tile (seja pegando elas do arquivo binário ou do dict do jogo) ele performa uma lista de checks na posição dessa tile e não faz nada com ela se ela não passar.
- Garantia no `mapdata` que nenhum acesso ou modificação seja feita fora das bounds com formulas usando a posição dada.

