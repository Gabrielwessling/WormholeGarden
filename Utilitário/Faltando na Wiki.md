### ⚠️ **Classes precisando de atenção**

• [ ] ``Slot``: declarada em ``Tile`` e ``Recipe``, mas não existe no diagrama.

• [ ] ``SlotType``: usado mas não definido.

• [ ] ``Inventory``: nenhum sistema para armazenar ``ItemData``.

• [ ] ``ItemContainer`` ou similar para baús, mochilas etc.

• [ ] ``ConstructionSystem``: responsável por aplicar ``Recipe``, validar regras, instanciar objetos.

• [ ] ``AIComponent`` / ``BehaviorTree`` / ``ActorLogic``: nenhum controle de comportamento de NPCs.

• [ ] ``Spawner`` / ``EcosystemController``: sistema que instancia fauna/flora com base em bioma.

• [ ] ``TimeSystem`` / ``WeatherSystem``: para ciclos diários, climas e simulação contínua.

• [ ] ``ObjectiveSystem`` / ``QuestComponent``: nada que lide com objetivos, tarefas, quests.

• [ ] ``InteractionSystem``: não existe controle geral para interações (``Furniture.onInteract`` é isolado).

• [ ] ``EventSystem`` (caso queira histórico ou reatividade no mundo).

---
### ⚠️ **Relações mal definidas**

• [ ] ``Tile → Biome``: Bioma afeta tile, mas não existe relação direta (campo ou sistema).

• [ ] ``Location → Position``: ``Location`` não tem ``Tile`` ou ``GridAnchor`` associado.

• [ ] ``Entity → AnchorMap``: ``Entity`` usa ``gridPosition``, mas não está claro se usa ``GridAnchor``.

• [ ] ``ConstructionPart``: não define ``quantidade`` de material.

• [ ] ``MaterialOption → ItemData``: não indica claramente como escolhe um item entre os compatíveis.

• [ ] ``Furniture → Tile``: ``occupiedTiles`` é declarado, mas ligação reversa (``Tile → Furniture``) não existe.

• [ ] ``spaceMap`` e ``Scene``: relação entre ambos é ambígua.

---
### ❓ **Decisões pendentes ou ambíguas**

• [ ] ``Entity.gridPosition`` é ``Vector2Int``, mas o sistema de ancoragem usa ``Vector2``.

• [ ] ``GridAnchor.occupant`` é único, mas múltiplas entidades podem ocupar uma posição. Precisa ``List<Entity>`` ou controle de camadas.

• [ ] ``Tile.building``: só aceita um por tile? E se tiver parede, piso, teto e móveis?

• [ ] ``Tile.isInterior``: quem define isso? Está como campo fixo ou derivado da ``Building``?

• [ ] ``handmadeLocations``: não está claro onde e como são posicionadas no ``Map``.

---
### 🔁 **Sistemas que precisam integração**

• [ ] Ligação clara entre ``Entity``, ``Tile``, ``GridAnchor``, ``AnchorMap``.

• [ ] Sistema de validação de construção (``PlacementRule.validate``) precisa ser aplicado por algum sistema.

• [ ] Falta de lógica de spawn e lifecycle para fauna/flora das biomes.

• [ ] Falta de relação entre ``ItemData`` e ``ItemEntity`` além do ``EntityPrefab``.