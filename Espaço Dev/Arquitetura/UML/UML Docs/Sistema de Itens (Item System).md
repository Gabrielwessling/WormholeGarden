Os itens são definidos por dados (`ItemData`) que referenciam seus atributos visuais e lógicos. Toda a lógica modular dos itens é controlada via **ItemComponents**, cada um responsável por um aspecto diferente do comportamento ou propriedades do item.

---
### ItemData

- **id**: Identificador único hierárquico (ex: `"metal.ingot.iron"`), usado para categorização e validações (ex: em `MaterialOption`).
- **tint**: Cor base usada para aplicar visual personalizado (ex: em texturas de construções).
- **components**: Lista de componentes (`ItemComponent`) que definem o comportamento e propriedades desse item.

---
### ItemComponent (base abstrata)

Classe base para todos os componentes de item. Permite que cada item tenha apenas os dados que importam para ele — sem herança pesada ou campos nulos.

```csharp
abstract class ItemComponent {
  public string UID;
}
````

---

### Tipos de `ItemComponent`

São exemplos por enquanto:

- **Consumable**  
    Define que o item pode ser consumido (comido, bebido, usado).
    
    - `isPercentageBased`: define se o consumo é em porcentagem do item (ex: 20%) ou em unidades inteiras (ex: 1 unidade).
        
- **Flammable**  
    Define combustibilidade.
    
    - `flammability`: quão fácil é de pegar fogo.
        
- **Metallic**  
    Propriedades físicas e químicas do material.
    
    - `reflectiveness`: brilho ou reflexão.
        
    - `meltingPoint`: ponto de fusão.
        
    - `hardness`: dureza.
        
    - `electricConductiveness`: condutividade elétrica.
        
    - `corrosionResistance`: resistência à corrosão.
        
- **Nutritional**  
    Define que o item é nutritivo (comida). Pode ser expandido depois com calorias, nutrientes, etc.
    
- **Weight**  
    Define peso do item, afeta carga e transporte.
    
    - `value`: valor numérico do peso.
        
- **Stackable**  
    Permite agrupar vários itens iguais em uma pilha.
    
    - `maxStack`: quantos itens cabem em uma pilha.
        
- **Equippable**  
    Define um item que pode ser equipado em membros de uma criatura ou personagem.
    
    - `neededLimbs`: membros necessários para equipar (ex: mão, boca, tentáculo).
        
    - `occupiedSlots`: slots que o item ocupa (ex: mão direita, cabeça).
        
- **ObjectiveValue**  
    Valor base que pode ser interpretado por culturas e sistemas econômicos diferentes.
    
    - `baseValue`: valor abstrato.
        
- **EntityPrefab**  
    Liga o item a uma entidade física no mundo, usada quando o item é solto ou colocado.
    
    - `entity`: uma instância de `ItemEntity`.
        

---

### Relações

- `ItemData` possui múltiplos `ItemComponent`.
    
- `MaterialOption` usa `ItemData` para definir quais itens são válidos como materiais.
    
- O sistema de construção e visual usa `ItemData.tint` e textura para aplicar o material visualmente.
    

---

### Exemplo: Espada de Ferro

```json
{
  "id": "metal.tool.sword.iron",
  "tint": "#c4c4c4",
  "components": [
    {
      "type": "Metallic",
      "hardness": 7.5,
      "meltingPoint": 1500
    },
    {
      "type": "Equippable",
      "neededLimbs": ["Hand"],
      "occupiedSlots": ["RightHand"]
    },
    {
      "type": "Weight",
      "value": 3.2
    },
    {
      "type": "ObjectiveValue",
      "baseValue": 120
    }
  ]
}
```

---
### Vantagens

- Sistema totalmente **data-driven**.
- Permite **composição emergente**: um item pode ser comestível, metálico e empilhável ao mesmo tempo.
- Facilidade de expandir com novos tipos de `ItemComponent`, sem afetar os já existentes.
- Ótima compatibilidade com inspeção em debug, ferramentas de editor e exportação/serialização.