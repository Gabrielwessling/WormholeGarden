## Visão Geral
Este documento detalha a arquitetura, requisitos e plano de implementação para um sistema robusto de tiles baseado em dados e a ferramenta Tile Manipulator. O objetivo é garantir integridade de dados, modularidade e máxima compatibilidade tanto para edição de mapas no editor quanto em runtime, com foco em suporte a modding e performance.

---

## 1. Modelo de Dados & Integridade

### 1.1 Estrutura do Tile

- **Tile**: Representa uma célula única na grid.
    - `Vector2Int Position` — Coordenada única na grid.
    - `bool isPassable` — Entidades podem atravessar este tile?
    - `bool isTerrain` — Este é um tile de terreno?
    - `TileHeights? Heights` — Opcional, para tiles esculpidos/pintados.
    - `List<Entity> Entities` — Entidades presentes neste tile.
    - `GridAnchor CenterAnchor` — Para posicionamento/lógica de entidades.
- **TileHeights**: Alturas dos vértices para escultura/pintura.
    - `float TopLeft, TopRight, BottomLeft, BottomRight`

### 1.2 Estrutura do Mapa

- **Map**: Contém todos os tiles e metadados.
    - `Dictionary<Vector2Int, Tile> tiles` — Armazenamento principal de dados. Posições duplicadas são proibidas.
    - `List<SerializableTile> serializedTiles` — Para serialização no Unity.
    - `Vector2Int mapSize` — Limites do mapa.
    - `bool tiledMap` — Bordas do mapa se conectam (comportamento tipo planeta).

### 1.3 Garantias de Integridade

- Nenhum tile duplicado em qualquer posição.
- Todas alterações passam por `Map.AddTile()` e `Map.RemoveTile()`.
- `SyncSerializedTiles()` deve ser chamado após qualquer alteração.
- Ao carregar, `OnEnable()` reconstrói `tiles` a partir de `serializedTiles`.
- Sem corrupção de dados (sem tiles duplicados, sem dados órfãos).

---

## 2. Lógica da UI do Tile Manipulator

### 2.1 Modos

- **Modos Principais:**
    - Adicionar (Add)
    - Remover (Remove)
    - Esculpir (Sculpt)
    - Modificar (Modify)
- **Modos Secundários:**
    - Adicionar/Remover: `Single`, `Rect`
    - Esculpir: `Paint`, `Vertex`
    - Modificar: _(nenhum no momento)_

### 2.2 Comportamento da UI

- Linha secundária atualiza conforme modo principal.
- Apenas opções secundárias válidas são exibidas.
- Eventos de UI direcionados à lógica correta.

### 2.3 Ações da Ferramenta

- **Modo Adicionar:**
    - _Single_: Arrastar para adicionar tiles ao longo do caminho (altura 0,0,0,0).
    - _Rect_: Arrastar para adicionar retângulo de tiles (altura 0,0,0,0).

- **Modo Remover:**
    - _Single_: Arrastar para remover tiles ao longo do caminho.
    - _Rect_: Arrastar para remover retângulo de tiles.

- **Modo Esculpir:**
    - _Paint_: Define todas as quatro alturas de um tile simultaneamente.
    - _Vertex_: Define vértices individuais do tile.

- **Modo Modificar:**
    - _(Expansão futura)_


---

## 3. Renderização Baseada em Dados & Geração de Malha (Mesh)

### 3.1 Geração de Malha

- Após qualquer alteração nos dados do mapa, regenerar a malha para todos os tiles visíveis.
- Combinar todos os tiles visíveis em uma única malha com materiais combinados.
- Atualizar malha apenas se dados mudarem (flag dirty ou hash).

### 3.2 Garantias de Renderização

- Mundo renderizado sempre corresponde a `Map.tiles`.
- Sem artefatos visuais por dessincronização de dados.

---

## 4. Persistência & Modding

### 4.1 Armazenamento de Mapas

- Armazenar dados como arquivos `.asset` (ScriptableObjects do Unity).
- Mapas devem ser carregáveis tanto no editor quanto em builds.
- O jogo deve escanear o diretório de build por novos mapas em runtime.
- Adicionar novo arquivo de mapa ao build deve disponibilizá-lo sem atualização de código.

### 4.2 Modularidade & Documentação

- Manter lógica de mapa, tile e malha em scripts separados e bem nomeados.
- Documentar estrutura de arquivos e fluxo de modding para contribuidores externos.

---

## 5. Etapas de Implementação

### 5.1 Camada de Dados

- Refatorar `Tile`, `TileHeights` e `Map` para integridade estrita.
- Adicionar checagens contra tiles duplicados.
- Garantir que todas alterações usem `Map.AddTile()`/`RemoveTile()`.
- Validar serialização/desserialização.

### 5.2 Camada de UI

- Implementar lógica de troca de modos no Tile Manipulator.
- Mostrar/ocultar opções secundárias conforme modo principal.
- Conectar UI às funções de edição de mapa.

### 5.3 Lógica das Ferramentas

- Implementar lógica de Adicionar/Remover/Esculpir para todos modos.
- Garantir que alterações atualizem dados e chamem `SyncSerializedTiles()`.

### 5.4 Camada de Renderização

- Atualizar gerador de malha para combinar todos tiles visíveis.
- Regenerar malha apenas quando dados mudarem.

### 5.5 Persistência & Modding

- Garantir salvamento de mapas como `.asset`.
- Implementar carregamento de mapas do diretório de build.
- Documentar estrutura de arquivos e processo de modding.

### 5.6 Testes

- Testar todos modos de ferramentas e integridade de dados.
- Testar salvamento/carregamento e fluxo de modding.

---

## 6. Referências

- `Tile.cs`, `Map.cs`, `MeshGenerator.cs`, `TileRendererUtility.cs`, `MapEditor/MapEditingUI.cs`, `MapEditor/RenderMap.cs`
- Serialização de ScriptableObject no Unity
- Melhores práticas de modding

---

## 7. Perguntas em Aberto / TODOs

- Como lidar com atualizações de propriedades (versionamento)?
- Devemos suportar undo/redo no editor?
- Como lidar com mapas muito grandes (chunking, streaming)?

---

_Documento vivo. Atualizar conforme implementação progride ou requisitos mudam._

---

### Notas de Tradução:

1. **Termos técnicos preservados**: Todos nomes de classes (`Tile`, `Map`), métodos (`AddTile()`, `SyncSerializedTiles()`), propriedades (`isPassable`, `Position`) e tipos (`Vector2Int`) mantidos em inglês conforme solicitado.
2. **Consistência**: Termos como _tile_, _mesh_, _modding_ e _runtime_ não foram traduzidos por serem padrão no contexto de desenvolvimento.
3. **Estrutura**: Formatação original (títulos, listas, destaques em `code blocks`) completamente preservada.
4. **Variantes regionais**: Utilizada grafia brasileira (ex: "integridade" em vez de "integridad" do português europeu).