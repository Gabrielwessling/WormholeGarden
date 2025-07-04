# Documentação do Sistema do Jogo

## Índice

- [[+World-Map Diagram]]
- [[+UML Diagram]]
- [[World System]]
- [[Geração Procedural de Terreno (Terrain Generation)]]
- [[Componentes de Mapa (Map Components)]]
- [[Sistema de Grid e Anchors (Grid Anchor System)]]
- [[Sistema de Entidades (Entity System)]]
- [[Sistema de Construção e Materiais (Construction Recipes & Materials)]]
- [[Sistema de Itens (Item System)]]

---
## Considerações Gerais

- Arquitetura modular e extensível, com separação clara entre dados do mundo, posicionamento, entidades e itens.  
- Uso do sistema de anchors permite posicionamento granular e regras sofisticadas.  
- Sistema de construção detalhado com receitas, materiais e validações, garantindo consistência visual e lógica.  
- Modelo baseado em componentes facilita o aumento da complexidade das entidades sem aumento de acoplamento.

![[+UML Diagram SVG.svg]]