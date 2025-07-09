Representa a estrutura base do mundo do jogo, suas cenas, mapas e o jogador.

- **World**  
  Contém uma lista de cenas (`scenes`), um mapa espacial (`spaceMap`), planetas (`planets`) e o jogador (`player`).

- **Scene**  
  Define uma cena específica com um tipo (`type`), mapa (`map`) e regras (`rules`).

- **Map**  
  Composto por tiles (`tiles`), mapa de âncoras para posicionamento (`anchors`), gerador procedural (`procGen`), localizações handmade (`handmadeLocations`) e todas as localizações (`locations`).

**Relações**  
- `World` possui múltiplas `Scene` e um `Map` espacial.  
- `Scene` referencia um `Map`.  
- `Map` possui tiles e um `AnchorMap`.  
- Cada `Tile` está associado a um `GridAnchor` central, que pode referenciar o tile.

Próximo: [[Sistema de Grid e Anchors (Grid Anchor System)]]