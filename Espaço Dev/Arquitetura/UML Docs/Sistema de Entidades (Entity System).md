Arquitetura baseada em entidades e componentes para os objetos do jogo.

- **Entity**  
  Unidade genérica com identificador, posição no grid e conjunto de componentes.

- **EntityComponent**  
  Classe base abstrata para estender funcionalidades de entidades.

- **Subclasses de Entity**  
  - `Actor`: entidades vivas com animação e estado.  
  - `Construction`: construções com slots e materiais.  
  - `Furniture`: móveis com tamanho, passabilidade e interação.  
  - `ItemEntity`: itens físicos no mundo com modelo e offset.

**Relações**  
- `Entity` é base para diversas subclasses.  
- `Entity` possui múltiplos `EntityComponent`.

Próximo: [[Sistema de Itens (Item System)]]