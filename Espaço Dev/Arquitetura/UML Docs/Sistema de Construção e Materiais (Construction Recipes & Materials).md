Define receitas de construção, materiais permitidos e regras.

- **Recipe**  
  Receita que define prefab, slots ocupados, partes, regras e rotações.

- **SlotOccupation**  
  Tipo de slot ocupado (ex: parede norte, chão).

- **ConstructionPart**  
  Partes da construção com materiais aceitos.

- **MaterialOption**  
  Opções de material com referência mínima (ex: "metal.ingot") e textura.

- **RotationData**  
  Suporta rotações da construção, com overrides nos slots.

- **PlacementRule**  
  Validações para posicionamento da construção.

**Relações**  
- `Construction` usa uma `Recipe`.  
- `Recipe` define slots, partes, regras e rotações.  
- Partes aceitam materiais específicos.  
- Rotação pode modificar ocupação dos slots.

Próximo: [[Sistema de Entidades (Entity System)]]