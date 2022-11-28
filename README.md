## Explicação de uma execução concorrente com resultado incorreto:
Ao compilar o código, com race condition, "sharedaccount", temos o seguinte resultado incorreto como exemplo:

![image](https://user-images.githubusercontent.com/63315782/204363249-0f865493-2ef2-4183-8e8a-b8894b546a69.png)

No código supracitado, temos um método chamado "deposita" e outro chamado "retira". Também temos 2 Threads: ThreadDeposita e ThreadRetira.
Estas operações de retirada de saldo e de depósito de saldo estão sendo executadas em diferentes threads, mas agindo sob um mesmo saldo; o que pode gerar interferência.
No caso do exemplo, uma execução concorrente que pode ter produzido tal resultado é a seguinte:
A ThreadDeposita chama o método deposita ao mesmo tempo em que a ThreadRetira chama o método retira. O valor do saldo inicial é 100. As ações intercaladas seguem a sequência abaixo.

1. ThreadDeposita: Recupera o saldo inicial = 100.
2. ThreadRetira: Recupera o saldo inicial = 100.
3. ThreadDeposita: Aumenta o saldo inicial em 100.
5. ThreadDeposita: Aumenta o saldo em 100.
7. ThreadDeposita: Aumenta o saldo em 100.
9. ThreadDeposita: Aumenta o saldo em 100.
11. ThreadDeposita: Aumenta o saldo em 100.
12. ThreadDeposita: Aumenta o saldo em 100.
13. ThreadDeposita: Aumenta o saldo em 100.
17. ThreadRetira: Diminui o saldo em 50. 
14. ThreadDeposita: Aumenta o saldo em 100.
18. ThreadDeposita: Armazena o resultado no saldo da conta; o saldo da conta agora é 850.
15. ThreadDeposita: Aumenta o saldo em 100.
16. ThreadDeposita: Aumenta o saldo em 100. 
4. ThreadRetira: Diminui o saldo em 50.
17. ThreadRetira: Diminui o saldo em 50. 
17. ThreadRetira: Diminui o saldo em 50. 
18. ThreadRetira: Armazena o resultado no saldo da conta; o saldo da conta agora é 900.
17. ThreadRetira: Diminui o saldo em 50. 

O resultado da ThreadDeposita é perdido e substituído pelo da ThreadRetira.
    
## Complementação da prática com algum tópico relacionado que tenha gerado dúvida e/ou interesse.:
### ***Dúvida***: 
Tem algum problema a ThreadRetira diminuir o saldo em 50 após ela já ter armazenado o resultado no saldo da conta (conforme ocorreu no caso acima)? ou é como se após o resultado ter sido armazenado na thread, o último "diminui saldo em 50" não fosse contabilizado? (muito embora o programa tenha solicitado que o valor "50"  fosse retirado 5 vezes e não 4).

***Resposta***: Não consegui encontrar a resposta no Google, mas acredito que depois que a ThreadRetira armazenou o valor, a última iteração do laço não acontece (ou ao menos é ignorada de alguma forma), com base no segundo programa da Tabela Compartilhada, pois, quando o programa da tabela compartilhada é executado de maneira sincronizada todos os 20 A's são mostrados no terminal, mas quando não é de maneira sincronizada, nem todos os A's são exibidos. 

***Referências***: https://github.com/AndreaInfUFSM/elc117-2022b/blob/main/praticas/java10/instructions/README03.md


### ***Tópico que gerou interesse***:  
Quais são algumas aplicações da programação concorrente?

***Resposta***:  Ferrovias, telegrafia, semáforos, estudo acadêmico de algoritmos simultâneos, contas bancárias, Java APIs.

***Referências***: 
- https://en.wikipedia.org/wiki/Concurrent_computing#Advantages
- https://begriffs.com/posts/2020-03-23-concurrent-programming.html
- https://www.oreilly.com/live-events/developing-concurrent-and-parallel-applications-in-java/0636920074038/0636920078821/
