Elementos que compõem e populam o mapa do jogo.

- **Location**  
  Áreas que agregam construções (`buildings`) e atores (`actors`).

- **Tile**  
  Unidade básica do mapa, com posição, passabilidade, slots, estado interior/exterior, construção e entidades presentes.

- **Building**  
  Construção formada por tiles, podendo ser hermética e categorizada.

- **Especializações de Location**  
  - `ArchaeologySite` (sítios arqueológicos)  
  - `DungeonEntrance` (entradas de masmorras)  
  - `Settlement` (assentamentos)

**Relações**  
- `Location` agrega `Building` e `Entity` (atores).  
- `Tile` pode conter `Building` e várias `Entity`.  
- `Building` é composta por múltiplos `Tile`.  
- `Biome` influencia tiles e buildings.

Próximo: [[Sistema de Construção e Materiais (Construction Recipes & Materials)]]