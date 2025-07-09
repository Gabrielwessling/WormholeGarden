O sistema foi projetado para permitir um **mundo composto por múltiplos mapas jogáveis**, cada um representado por uma **cena do Unity** com dados adicionais (tiles, entidades, metadados etc). O mundo pode transitar entre mapas com ou sem descarregar cenas anteriores, suportando estruturas como:
- Mapas persistentes carregados additive
- Transições entre cenas com posição e rotação
- Áreas handmade e procedural integradas
- Hibernação de mapas fora de foco

---
##  Estrutura Geral:
####  `World`
Responsável por gerenciar o estado global do jogo:
- Lista de `Map`
- Referência ao `player`
- Métodos para carregar, descarregar, hibernar mapas
- Gerencia transições entre cenas (`LoadSceneWithTransition`)
####  `Map`
Representa um mapa jogável, atrelado a uma cena Unity. Contém:
- `name`: nome único do mapa
- `sceneAsset`: referência à cena Unity (.unity)
- `tiles`: grade de `Tile`
- `anchors`: `AnchorMap` (sistema de slots para entidades)
- `entities`: lista de `Entity` presentes no mapa
- `locations`: áreas nomeadas e agrupadas
- `handmadeLocations`: blocos do mapa que não devem ser sobrescritos pelo procgen
- `procGen`: gerador procedural (para regiões fora das handmade)
- `metaData`: configurações ambientais e flags

> Toda cena de mapa carrega um objeto `Map` que representa os dados daquela cena.

---
##  Ciclo de Vida de um Mapa:

- **Criação**:
    - Nova cena Unity é criada
    - Instancia-se um `Map` com grid de tiles e dados básicos
- **Carregamento**:
    `World.LoadMap("base_caverna");`
    - `sceneAsset` é carregado com `LoadSceneAdditive`
    - `Map.Load()` instancia todos os tiles, entidades, anchors etc
- **Ativação e uso**:
    - Cena torna-se jogável
    - Entidades são atualizadas
    - Player pode interagir e movimentar-se
- **Transição para outro mapa**:
    - Uma entidade com `SceneTeleportEvent` é ativada
    - Chamada para `World.LoadSceneWithTransition(...)`
- **Hibernação de mapa anterior**:
    - GameObject root do mapa anterior é desativado
    - Cena permanece carregada mas inativa
    - Pode ser reativada instantaneamente
- **Salvamento**:
    - `Map.Save()` persiste os dados da grid, entidades, meta etc

---
##  Tiles e Anchors

### `Tile`

Cada unidade do mapa. Contém:
- `position`: coordenada na grade
- `vertexHeights[4]`: altura dos 4 vértices do tile (para relevo)
- `passable`: se é atravessável
- `isInterior`: se é parte de um espaço fechado
- `anchors`: referências aos `GridAnchor` que passam por esse tile
- `entities`: entidades localizadas diretamente sobre o tile
### `AnchorMap`

Gerencia dinamicamente os pontos de ancoragem (slots):
- `GridAnchor`s com posição precisa (ex: centro, borda, canto)
- Ocupantes (entidades posicionadas naquele anchor)
- Usa `getOrCreateAnchor(pos)` para criar slots sob demanda

> Paredes, móveis, objetos são posicionados via `AnchorMap` (não diretamente nos tiles).

---
##  Estados de Cena e Hibernação

| Estado    | Descrição                                          |
| --------- | -------------------------------------------------- |
| Ativa     | Cena carregada, root ativo, lógica rodando         |
| Hibernada | Cena carregada, root desativado, lógica desativada |
| Unloaded  | Cena descarregada                                  |

---
##  Sistema de Editor

O editor é uma cena jogável standalone que permite aos devs criarem e gerenciarem mapas.

O editor permite:
- Criar mapas (nova cena + grid)
- Pintar altura dos `Tile.vertexHeights`
- Posicionar entidades via ferramentas (paredes, móveis, luzes…)
- Colocar eventos de transição (teleporter)
- Editar `MapMetaData` e propriedades gerais
- Salvar mapas como jogáveis
- Exportar o mapa como arquivo

Next: Implementando o [[Editor]]