layout: oishi
title: Aumentando a confiança do time com DevOps
date: 2015-04-01 13:43:57
tags:
- DevOps
---
Um dos resultados da adoção do DevOps é a [experimentação contínua](http://www.ibm.com/developerworks/br/library/se-devops/part1/). E para um time realizar experimentos, precisa de confiança. Recentemente ao [provisionar um servidor oracle](http://acfreitas.com/2015/03/Provisionando-um-servidor-Oracle-com-Vagrant-e-Shell/), presenciei o encorajamento do time e aumento da confiança. 

<!-- more -->
### Cenário 

Como citado no livro *The Visible Ops Handbook*: servidores configurados manualmente são "obras de arte".

E de fato tínhamos uma obra de arte em mãos. Um servidor no qual o time não tinha coragem de fazer qualquer mudança. Neste servidor estavam os banco de teste, desenvolvimento, homologação, treinamento e produção. E eis aqui os principais problemas: 

* Por ser um servidor antigo, as informações de configuração se perderam no tempo;
* Era impossível reproduzir exatamente suas configurações e reproduzir um novo servidor;
* Qualquer experimentação poderia parar a produção;
* As informações existentes sobre as configurações era de conhecimento de poucas pessoas do time de operações;

### Solução

Um dos aprendizados do DevOps é que repetição e prática são pré-requisitos para dominar algo. E um dos princípios é  automatizar os processos manuais. Logo, a solução foi automatizar o [provisionamento do servidor de banco de dados](https://github.com/acfreitas/oraclebox). 

* Com isso poderíamos repetir o processo várias vezes;
* As configurações eram versionadas. Sendo assim, era possível voltar em um estado anterior, caso fosse necessário;
* O time tinha um novo servidor de testes e desenvolvimento, isolados da produção;
* Experimentos poderiam ser feitos. Pois caso algo desse errado, era possível reverter; 
* As informações eram de conhecimento de todos, já que estavam nos scripts; 
* O time de QA e Dev tinha autonomia para realizar novos experimentos; 

### Resultado 

Após apresentar para o time, alguns, principalmente os desenvolvedores, se sentiram encorajados para realizar os experimentos que há tempos desejavam. Pois tinham confiança que poderiam realizá-los e aprender com o fracasso ou sucesso de cada um e que isso não teriam impacto negativo. O time tinha confiança que uma otimização no servidor de desenvolvimento não causaria regressão no servidor de produção. E caso a otimização fosse positiva, poderia ser replicada nos outros servidores. 

E você, tem presenciado alguma experiência que aumentou a confiança do time? 