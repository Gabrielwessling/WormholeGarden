Este sistema fornece um modelo granular e preciso para posicionamento de entidades, construções e interações dentro do grid do mapa.  
Ele separa a noção de "Tile" (lógica de terreno) da noção de "posição física ocupável", permitindo representações intermediárias como bordas e interseções.
### GridAnchor
Representa um ponto de ancoragem dentro do grid, com posição decimal (`Vector2`) e tipo (`GridAnchorType`).
- **position**: um `Vector2` que representa uma posição no espaço do mapa. Pode ser inteira (centro do tile), meia (borda) ou intermediária (interseção).  
  Exemplos:
  - (2.0, 3.0) → centro do tile  
  - (2.5, 3.0) → borda horizontal entre dois tiles  
  - (2.5, 3.5) → interseção entre quatro tiles
  
- **anchorType**: tipo de âncora (`Tile`, `Border`, `Intersection`), definindo sua função e regras de ocupação.
- **occupant**: referência opcional para uma `Entity` que está ocupando esse ponto. Pode ser usada para detectar colisões, construção, ocupação, etc.

```csharp
class GridAnchor {
  public Vector2 position;
  public GridAnchorType anchorType;
  public Entity? occupant;
}
````
---
### GridAnchorType
Enumeração que define o papel geométrico da âncora:
```csharp
enum GridAnchorType {
  Tile,         // Ponto central de um tile inteiro
  Border,       // Ponto entre dois tiles (x+0.5, y) ou (x, y+0.5)
  Intersection  // Ponto entre quatro tiles (x+0.5, y+0.5)
}
```
- **Tile**: usado para construções, NPCs, objetos centrais.
- **Border**: usado para paredes, cercas, portões, ou ocupações que ficam entre tiles.
- **Intersection**: usado para postes, cruzamentos, detalhes estruturais, partículas fixas no chão.
---
### AnchorMap
Gerencia todas as `GridAnchor` disponíveis no mapa. Serve como um índice espacial dinâmico.
- **anchors**: dicionário (`Dictionary<Vector2, GridAnchor>`) que mapeia posições para âncoras.
- **occupiedPositions**: conjunto de posições ocupadas, otimiza consultas de colisão e posicionamento.
- **getAnchor(pos: Vector2)**: retorna a `GridAnchor` na posição, se existir.
- **occupy(pos: Vector2, entity: Entity)**: registra uma entidade como ocupando um ponto.
- **release(pos: Vector2)**: libera a ocupação de um ponto.
- **isOccupied(pos: Vector2)**: retorna `true` se a posição estiver ocupada por alguma entidade.
```csharp
class AnchorMap {
  Dictionary<Vector2, GridAnchor> anchors;
  HashSet<Vector2> occupiedPositions;

  GridAnchor getAnchor(Vector2 pos);
  void occupy(Vector2 pos, Entity entity);
  void release(Vector2 pos);
  bool isOccupied(Vector2 pos);
}
```

---
### Exemplo de Uso
- Uma **parede** pode ser colocada na posição (2.5, 3.0), que é um `GridAnchorType.Border`.
- Um **poste de luz** pode ser instanciado em (4.5, 7.5), uma interseção.
- Um **personagem** está posicionado na âncora de tipo `Tile` em (5.0, 8.0).
- Uma **mesa grande** pode ocupar múltiplas âncoras tipo `Tile`.
---
### Relações
- `Map` possui uma instância de `AnchorMap` (`map.anchors`).
- `AnchorMap` gerencia todos os `GridAnchor` de um mapa.
- Cada `GridAnchor` pode opcionalmente estar **ocupado** por uma `Entity`.
- Cada `Tile` tem uma `GridAnchor` do tipo `Tile`, associada como `centerAnchor`.
---
### Vantagens
- Permite modelagem precisa para construções que não se limitam ao centro de tiles.
- Flexibiliza a lógica de colisão e ocupação sem precisar subdividir o tile.
- Otimiza interações como "coloque a parede entre estes dois tiles" ou "posicione o poste no cruzamento".
---
### Futuras Expansões Possíveis
- Suporte a múltiplos ocupantes por âncora.
- Regras de priorização ou conflito em ocupações simultâneas.
- Utilitários de Posição e Âncoras

---
## 🧭 Utilitários de Posição e Âncoras [[Sistema de Grid e Anchors (Grid Anchor System)]]

Esta nota complementa o [[Sistema de Grid e Anchors (Grid Anchor System)]], adicionando funções práticas para navegar entre tiles e anchors, além de traçar caminhos em estruturas como paredes ou fios.

---

### 🔹 `GetTileAtPosition(Vector2 position): Tile?`

Retorna a `Tile` mais próxima de uma posição do tipo `Vector2`.

- Arredonda para `Vector2Int` usando `floor()`.
- Ideal para localizar tiles a partir da posição de uma âncora, entidade, clique ou raycast.

---

### 🔹 `GetTilesAtAnchor(GridAnchor anchor): List<Tile>`

Retorna todas as `Tile` que estão conectadas à âncora fornecida.

- `Tile`: retorna uma única tile.
- `Border`: retorna as duas tiles adjacentes.
- `Intersection`: retorna as quatro tiles em volta.

---

### 🔹 `GetAnchorsAroundTile(Tile tile): List<GridAnchor>`

Retorna as âncoras ao redor de uma `Tile`, incluindo:

- 1 centro (tipo `Tile`)
- 4 bordas (tipo `Border`)
- 4 interseções (tipo `Intersection`)

Perfeito para lógica de construção, highlight e pathfinding.

---

### 🔹 `GetAnchorsAroundPosition(Vector2 position): List<GridAnchor>`

Mesma ideia da anterior, mas parte de uma `Vector2`, encontrando a `Tile` mais próxima automaticamente.

---

## 🔧 Traçar Caminho entre Interseções

### `TraceAnchorsBetweenIntersections(List<GridAnchor> intersections): List<GridAnchor>`

Função que retorna o caminho de âncoras intermediárias entre uma sequência de `Intersection` anchors.

Usado para:
- Colocar automaticamente paredes
- Traçar fiação, trilhos, túneis
- Conectar pontos do mundo com estrutura física

---

### Exemplo visual

```

(1.5,1.5)──(2.5,1.5)
					\
						(3.5,2.5)


````

Resultado: exemplo de colocação de paredes!
- (1.5,1.5) `Intersection`
- (2.0,1.5) `Border` - Uma parede vai aqui, virada horizontalmente
- (2.5,1.5) `Intersection`
- (3,2) `Tile` - Outra vai aqui, de forma diagonal NW-SE
- (3.5,2.5) `Intersection`
#### Lógica por trecho

- Se o movimento for **horizontal ou vertical**, retorna 1 `Border`
- Se for **diagonal**, retorna 1 `Tile` central
### Requisitos

- Âncoras devem ser do tipo `Intersection`
- As posições devem estar conectadas por distância 1
- A `AnchorMap` precisa conter todos os anchors necessários

---

### Vantagens

- Permite modelagem precisa de paredes, cercas, redes
- Usa os tipos corretos de `GridAnchor` para ocupação lógica e física
- Compatível com visualização procedural e lógica de simulação

Próximo: [[Geração Procedural de Terreno (Terrain Generation)]]