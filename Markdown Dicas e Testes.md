
---
# Headers
Isso é uma H1
===
lorem ipsum

Isso é uma H2
---
lorem ipsum

# Isso é uma H1 #
lorem ipsum

## Isso é uma H2 ##################
lorem ipsum

	Você não precisa fechar os cabeçalhos, como colocar # depois, mas se quiser mesmo assim, nem precisa da mesma quantidade de "#" como no início

===
Heading===
```
===
Heading===
```
↑ Isso não funciona e ainda chama algum tipo de função de highlights, vamos pesquisar sobre isso agora...

---
# Highlights
Lorem ==Highlight== Ipsum
```
Lorem ==Heading== Ipsum
```
↑ Assim que se faz highlights

---
# Citações ou Notas

> Lorem
ipsum

dolor

```
> lorem
ipsum

dolor
```

	Aparentemente tu precisa só de um > pra definir uma citação, se tu não quiser que as próximas linhas estejam nela, tu precisa de um espaço vazio entre elas.

Mas se tu quiser pode fazer assim:
> Citação
> Outra citação começando com >
Uma citação que não começa com >
>> Nota dentro da nota kkkk

E agora, duas linhas depois, nada de citação!

```
> Citação
> Outra citação começando com >
Uma citação que não começa com >
>> Nota dentro da nota kkkk

E agora, duas linhas depois, nada de citação!
```

Exemplo da própria documentação do markdown, n sei se é oficial [Link](https://daringfireball.net/projects/markdown/syntax)

> ## This is a header. 
> 
> 1. This is the first list item. 
> 2. This is the second list item. 
> 
> Here's some example code: 
> 
>     `return shell_exec("echo $input | $markdown_script");`

```
> ## This is a header. 
> 
> 1. This is the first list item. 
> 2. This is the second list item. 
> 
> Here's some example code: 
> 
>     return shell_exec("echo $input | $markdown_script");
```
- 4 espaços criam esse bloco de código, também pode usar "> > content".


