layout: oishi
title: Qual a diferença entre entrega contínua e deploy contínuo?
date: 2015-03-20 09:04:23
tags:
---

Nos últimos anos se tem falado muito sobre [entrega contínua](http://martinfowler.com/books/continuousDelivery.html) e [deploy contínuo](http://guide.agilealliance.org/guide/cd.html) como importantes práticas ágeis. Porém tenho percebido que muitos confundem os objetivos e formas de implantar cada uma das práticas (e confesso, que também já me enganei). Pois existem algumas coisas incomum entre as duas práticas, como: são uma extensão da [integração contínua](http://www.martinfowler.com/articles/continuousIntegration.html), dependem de automação, da colaboração entre as equipes de operações, desenvolvimento e QA, tem um pipeline, visam diminuir os ciclos de feedbacks, aumentam os feedbacks, diminuem os riscos de uma entrega, aumentam a qualidade e confiança do projeto, aumentam a frequência de entrega de valor ao usuário, diminuem os ciclos de respostas as mudanças e etc.

<!-- more -->
Segundo a [ThoughtWorks](http://www.thoughtworks.com/),

> Entrega Contínua é uma disciplina de desenvolvimento na qual software é construído de tal maneira que o mesmo *pode ser colocado em produção a qualquer momento*.

Já Jez [Humble](https://twitter.com/jezhumble) define deploy contínuo como: 

> Deploy contínuo é a prática de ir para produção de forma automatizada sempre que um novo commit passar com sucesso pela pipeline de entrega, *sem nenhum passo manual*.

A grande diferença fica por conta da decisão de quando disponibilizar uma nova versão para o cliente. Na entrega contínua você sempre terá uma versão *candidata* para publicação após cada commit passar pela pipeline de integração contínua e a decisão parte do negócio, onde o processo de entrega é automatizado e depende de uma ação humana para colocar uma nova versão no ar. Tal ação humana, normalmente, é a escolha da versão e o ambiente que será feito o deploy (testes, homologação ou produção, por exemplo). 

Já na implantação contínua você tem vários deploys por dia, horas e até minutos, um bom exemplo disso foi implantado no [Flickr](http://pt.slideshare.net/jallspaw/10-deploys-per-day-dev-and-ops-cooperation-at-flickr) e [IMVU](http://timothyfitz.com/2009/02/10/continuous-deployment-at-imvu-doing-the-impossible-fifty-times-a-day/) . E isso, normalmente, é feito se um commit passar por todos os estágios da pipeline de implantação.  

E como decidir qual abordagem utilizar? Para isso você pode fazer algumas perguntas, como: 

* Alguma ação humana é necessária? Exemplo: Testes exploratórios, testes de confirmação, apresentação para o cliente, aprovação do cliente e etc. 
* Eu estou entregando valor a cada versão? 
* A implantação de uma nova versão depende do negócio? Exemplo: implantações só podem ser feitas depois do horário comercial. 

Últimamente tenho trabalhado na implantação da entrega contínua e, sem dúvidas, é um esfoço muito benéfico para todos envolvidos no projeto.

Abraços! 