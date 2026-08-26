# Do Spec-Driven Development às Living Specifications em Projetos Java

Os agentes de código mudaram a velocidade com que conseguimos produzir software. Mas produzir código mais rápido não significa, automaticamente, produzir software melhor.

Conforme comecei a utilizar agentes com mais frequência no desenvolvimento de software no mundo real, uma pergunta se tornou cada vez mais importante: **como dar liberdade suficiente para que um agente seja útil sem perder a intenção de engenharia?**

Tentar controlar cada linha de código gerada pelo agente elimina boa parte do valor que ele pode oferecer. Nesse ponto, talvez fosse mais rápido escrever o código nós mesmos.

Por outro lado, apenas descrever um resultado e aceitar qualquer implementação que o produza nos aproxima perigosamente do *vibe coding*. 

Deve existir algo entre esses dois extremos.

Uma resposta promissora para essa questão parece estar nas especificações. Elas poderiam tornar a intenção explícita antes da implementação, estabelecer limites para o agente e criar algo contra o qual o software resultante pudesse ser avaliado.

Isso é o que chamamos de **Spec-Driven Development**.

Mas, depois de experimentar essa abordagem, outra pergunta começou a parecer mais interessante do que simplesmente como usar uma especificação para conduzir uma mudança:

**O que acontece com a especificação depois que a mudança termina?**

---

## A promessa do Spec-Driven Development

Uma das abordagens que influenciaram fortemente essa jornada veio da minha querida Oracle ACE e Java Champion [**Loiane Groner**](https://loiane.com/), por meio de seu artigo [*"Vibe Coding, But Production-Ready: A Specs-Driven Feedback Loop for AI-Assisted Development."*](https://loiane.com/2026/03/vibe-coding-with-specs-driven-feedback-loops/)

O que chamou minha atenção foi a ideia de trazer disciplina de engenharia para o desenvolvimento assistido por IA sem abrir mão da produtividade que os agentes de código poderiam proporcionar.

O ciclo avançava progressivamente da intenção do produto até a implementação e sua verificação:

<p align="center">
<img width="400px" src="https://raw.githubusercontent.com/dearrudam/articles/refs/heads/main/sdd/sdd4j/sldd-flow-pt-br.png" />
</p>

Em vez de entregar um prompt isolado para o agente e imediatamente pedir código, cada etapa produzia contexto para a próxima.

Conversando com um amigo da comunidade e também contribuidor opensource, [**Matheus Oliveira**](https://www.linkedin.com/in/omatheusmesmo/), decidimos colocar essas ideias em prática.

Materializamos esse ciclo em um conjunto de Agent Skills que chamamos de [**SLDD**](https://github.com/soujava/sldd-skills).

E a primeira descoberta importante foi ver um workflow como esse funcionando do início ao fim.

O workflow nos trouxe estrutura. Com isso, premissas poderiam ser questionadas antes da implementação, decisões técnicas poderiam ser revisadas e os testes passaram a fazer parte do caminho entre intenção e código, em vez de algo adicionado posteriormente.

Mas experimentar uma ideia faz mais do que simplesmente nos dizer se ela funciona.

A experimentação expõe situações que não havíamos previsto e, mais importante, nos permite fazer novas perguntas. E foi exatamente o que aconteceu com o SLDD.

Por isso, sou particularmente grato à Loiane.

Seu artigo fez mais do que nos apresentar uma abordagem interessante. Ele nos incentivou a experimentá-la. Algumas das perguntas que acabaram moldando esta jornada só se tornaram visíveis porque primeiro tivemos a oportunidade de colocar essas ideias em prática.

**Obrigado, Loiane, por compartilhar suas ideias e nos provocar a experimentá-las.**

---

## O que aprendemos colocando tudo isso em prática

À medida que continuamos experimentando com o SLDD e explorando outras abordagens de Spec-Driven Development, incluindo including [**OpenSpec**](https://openspec.dev/) e [**GitHub Spec Kit**](https://github.github.com/spec-kit/), começamos a prestar menos atenção nas etapas individuais de cada workflow e mais atenção no modelo por trás delas.

Especificações podem ser extremamente úteis enquanto uma mudança está acontecendo.

Elas tornam a intenção explícita, preservam decisões, fornecem contexto ao agente de código e criam pontos de verificação antes que a implementação avance demais na direção errada.

Mas esses benefícios também trazem trade-offs.

Múltiplos artefatos de especificação e design significam mais contexto para produzir, persistir, recuperar e interpretar. Mais etapas no workflow também podem significar mais transições humanas: gerar, revisar, aprovar, continuar.

Em determinado momento, percebemos algo desconfortável em nosso próprio experimento:

**o humano estava se tornando parte da máquina de estados do workflow, ou seja, estava se tornando o gargalo do processo.**

Em vez de participar somente nos momentos em que julgamento de engenharia era necessário, algumas vezes estávamos simplesmente permitindo que o processo avançasse de um estado para outro.

Mas uma pergunta ainda mais importante surgiu depois que o workflow terminava.

Imagine que uma especificação conduziu com sucesso uma funcionalidade até produção.

Algumas semanas depois, alguém corrige um pequeno bug diretamente na implementação.

Os testes passam, o código é revisado e a mudança é implantada.

Mas a especificação original nunca é alterada.

No fim, o software evoluiu enquanto sua especificação continua descrevendo uma realidade anterior.

**O ciclo de vida de uma mudança é bem menor que o de um software, que tende a permanecer em constante evolução.**

Isso mostra que uma especificação pode ser excelente para conduzir uma mudança e, ainda assim, perder utilidade depois que essa mudança passa a fazer parte do sistema.

E isso traz outro questionamento interessante:

**Como uma especificação pode continuar útil depois que essa mudança passa a fazer parte do software?**

---

## Quando a especificação não é a fonte da verdade

Spec-Driven Development é frequentemente apresentado em torno da ideia de que a especificação se torna a *source of truth*.

Como expressão da intenção do software, essa pode ser uma ideia interessante, mas existe um detalhe prático um pouco incômodo:

**Especificações não executam em produção. Código executa.**

Isso não torna a especificação inútil. Significa apenas que ela representa um tipo diferente de verdade:

-   A especificação descreve o comportamento pretendido.
-   O código implementa o comportamento real.
-   Os testes fornecem evidências executáveis conectando os dois.

Isso nos leva a um modelo que se tornou cada vez mais importante:

<p align="center">
<img width="400px" src="https://raw.githubusercontent.com/dearrudam/articles/refs/heads/main/sdd/sdd4j/spec-test-code-pt-br.png" />
</p>

E sim, as setas aqui importam, porque software evolui nas duas direções:

-   Às vezes, a implementação viola acidentalmente um requisito     existente e o código precisa convergir em direção à especificação.
-   Em outros momentos, a implementação evolui intencionalmente e é a especificação que precisa acompanhar essa mudança.

Esse modelo permite transformar a especificação em algo que podemos chamar de **Living Specification**.

Isso não quer dizer que ela vai se atualizar "magicamente", pois *drifts* ainda podem acontecer. A ideia tem um objetivo mais pragmático:

**tornar a divergência visível e a convergência barata.**

Essa ideia mudou aquilo que estávamos procurando.

Queríamos especificações próximas o suficiente da implementação para que desenvolvedores e agentes naturalmente as encontrassem.

Queríamos requisitos precisos o suficiente para serem testáveis.

E queríamos que a especificação descrevesse a **promessa atual de um componente de software**, e não apenas preservasse a história da mudança que o criou.

Foi então que encontrei o SBCE.

---

## SBCE: aproximando a especificação do código

[**SBCE**](https://sbce.space/), pronunciado *"space"*, é um workflow de Spec-Driven Development criado pelo Java Champion [**Adam Bien**](https://www.linkedin.com/in/adam-bien/), construído em torno do estilo arquitetural [**Boundary-Control-Entity (BCE)**](https://bce.design/).

Uma de suas ideias imediatamente se conectou às perguntas que estávamos fazendo:

**a especificação vive junto do código.**

No SBCE, cada *Business Component* (BC) mantém sua especificação dentro do `package-info.java`, como Javadoc no formato Markdown (Java 23+), localizado em seu próprio pacote.

Isso elimina a necessidade de manter uma árvore paralela de especificações em outra estrutura distante do código.

De fato, isso não transforma magicamente a documentação em algo executável nem elimina o *drift*, mas muda sua localização.

A especificação passa a fazer parte da mesma vizinhança estrutural onde o código vive, facilitando a localização das partes envolvidas tanto por desenvolvedores quanto por agentes de código ao trabalhar naquele componente.

Outra ideia importante foi expressar o comportamento de um Business Component por meio de **requisitos testáveis e rastreáveis**, utilizando estruturas inspiradas em EARS.

No SBCE, esses requisitos são expressos em inglês.

Em vez de um requisito ambíguo como:

> The checkout should work correctly.

podemos expressar algo observável:

> When a checkout is requested for an empty cart, the Business Component shall reject the request.

Ao dar um identificador estável para esse requisito, podemos estabelecer uma relação como:

![](https://raw.githubusercontent.com/dearrudam/articles/refs/heads/main/sdd/sdd4j/req-test-comp-pt-br.png)

O agente continua tendo liberdade para raciocinar sobre a implementação, mas o comportamento esperado passa a ter uma fronteira determinística de verificação.

O SBCE também reforçou outra lição para nós:

**simplicidade importa.**

Seu workflow principal é deliberadamente pequeno, fornecendo dois modos:

-   **`/sbce new`**
    -   declara um *Business Component*, escrevendo sua especificação no `package-info.java` de seu pacote;
-   **`/sbce apply`**
    -   trabalha na convergência dos *drifts* detectados entre especificação e código.

Outras preocupações, como convenções de código, podem ser delegadas a skills que se compõem, em vez de expandir continuamente um enorme conjunto de instruções.

Durante a exploração do SBCE, tive a oportunidade de conversar com Adam sobre a possibilidade de levar essa ideia para projetos Java que utilizam outras stacks além de MicroProfile, como Spring Boot, entre outras.

A conversa foi muito interessante e esclareceu uma distinção importante sobre o SBCE:

**SBCE é opinativo em relação à arquitetura, mas extensível em relação à tecnologia.**

A arquitetura BCE fornece a fundação arquitetural e o significado de um Business Component para o SBCE.

Adam descreveu a possibilidade de compor skills adicionais para trabalhar com outras stacks usando o conceito de **Inversion of Control** no processo.

Isso significava que suportar Spring Boot não exigia necessariamente modificar o SBCE. Uma skill específica para Spring Boot poderia mapear BCE para essa stack.

Mas isso também revelou qual era, de fato, o nosso problema.

A grande maioria dos projetos Java com os quais trabalhamos é *brownfield*. Além disso, muitas dessas aplicações utilizam Spring Boot e **não adotam BCE** como arquitetura principal.

Algumas utilizam *package by feature*, outras *package by layer* e, em muitos casos, encontramos uma mistura de decisões arquiteturais acumuladas ao longo dos anos.

Para levar essas ideias a esses projetos, precisaríamos fazer mais do que trocar a stack tecnológica: precisaríamos também lidar com arquiteturas além de BCE.

E ali surgiu uma nova oportunidade de experimentação.

---

## SDD4J: Adaptando-se a mais de uma arquitetura em projetos Java

A pergunta que queríamos explorar era relativamente simples:

**E se o significado de um Business Component pudesse se adaptar à arquitetura do projeto Java?**

Em vez de fazer com que o workflow principal de especificação compreendesse todas as arquiteturas possíveis, introduzimos a ideia de **architecture adapters**.

Conceitualmente:

![](https://raw.githubusercontent.com/dearrudam/articles/refs/heads/main/sdd/sdd4j/sdd4j-arch-adapter.png)

O núcleo continua preocupado com especificações, requisitos testáveis, rastreabilidade e convergência.

O adapter responde a uma pergunta arquitetural:

**Onde está o Business Component neste projeto?**

-   Em *package by feature*, o próprio pacote da feature pode naturalmente fornecer essa fronteira.
-   Em BCE, a própria estrutura BCE já fornece essa delimitação.
-   Em *package by layer*, uma capacidade de negócio pode estar distribuída entre diversos pacotes técnicos, exigindo outra estratégia de mapeamento.

Isso é particularmente importante no desenvolvimento *brownfield*.

Não queríamos que a adoção de Living Specifications exigisse primeiro a reorganização de uma aplicação existente.

Logo, **o workflow deveria reconhecer o projeto como ele já é.**

Em torno desse modelo, criamos o workflow [SDD4J](https://github.com/soujava/agent-skills). Ele fornece um pequeno conjunto de operações:

-   **`/sdd4j setup`** estabelece o contexto do projeto, como arquitetura e idioma utilizado pelas especificações.
-   **`/sdd4j new`** cria a especificação de um novo Business Component e seus requisitos testáveis.
-   **`/sdd4j apply`** procura divergências entre especificação, testes e implementação e ajuda a convergir essas partes.
-   **`/sdd4j verify`** procura evidências executáveis de que os requisitos declarados continuam representados pelos testes e pelo comportamento do software.

O idioma configurado durante o `setup` também é utilizado na escrita dos requisitos EARS.

Isso permite que um projeto configurado para PT-BR, por exemplo, mantenha suas especificações e seus requisitos nesse idioma.

Assim, o mesmo requisito apresentado anteriormente poderia ser expresso em um projeto SDD4J configurado para PT-BR como:

> Quando um checkout for solicitado para um carrinho vazio, o Business Component deverá rejeitar a solicitação.

Mantemos, portanto, a mesma intenção:

![](https://raw.githubusercontent.com/dearrudam/articles/refs/heads/main/sdd/sdd4j/req-test-comp-pt-br.png)

mas no idioma configurado para o projeto.

Essas operações também não foram pensadas como uma máquina de estados obrigatória pela qual toda alteração de código precisa passar.

Uma pequena correção de bug talvez não justifique um workflow elaborado com agentes.

Às vezes, o desenvolvedor deve simplesmente fazer a mudança.

Às vezes, o agente deve implementá-la.

Em outros casos, o papel mais útil do agente pode ser inspecionar o resultado posteriormente e identificar testes ausentes ou *drift* na especificação.

O objetivo não é criar cerimônia.

O objetivo é preservar estrutura suficiente para que humanos e agentes consigam compreender **o que um Business Component promete fazer** e verificar se o software continua cumprindo essa promessa.

Outras Agent Skills podem ser compostas ao redor desse núcleo para fornecer *guardrails* de engenharia específicos do projeto.

A ideia não é ensinar ao agente tudo aquilo que ele já conhece sobre Java, Spring, testes ou design de software.

É fornecer o **delta**: as decisões e restrições que importam especificamente para aquele projeto.

Dê liberdade suficiente para que o agente possa raciocinar.

Dê restrições suficientes para que ele permaneça alinhado ao projeto.

E mantenha as mudanças pequenas o suficiente para que um engenheiro humano ainda consiga compreendê-las e revisá-las.

---

## Não existe bala de prata

Seria fácil contar essa história como uma progressão: experimentamos uma abordagem, encontramos limitações, descobrimos outra, melhoramos essa ideia e finalmente chegamos ao SDD4J.

Essa seria a conclusão errada.

Nós não eliminamos os trade-offs.

**Escolhemos trade-offs diferentes.**

O SBCE deliberadamente se ancora na arquitetura **BCE** e é agnóstico em relação à stack tecnológica, o que significa que pode ser utilizado com pilhas Java ou qualquer outra, como a de *web components*.

Isso lhe dá uma definição clara de Business Component e permite que preocupações específicas de tecnologia variem por meio de skills.

O SDD4J deliberadamente se ancora no **ecossistema Java** aceitando diversas abordagens arquiteturais.

Isso nos oferece mecanismos nativos como `package-info.java` e Javadoc, enquanto permite que a interpretação arquitetural de um Business Component varie por meio de adapters.

Nenhum deles é universalmente melhor.

Para um projeto já organizado em torno de BCE e, independente da stack tecnológica, SBCE pode ser a escolha mais simples e natural.

Para um projeto Java existente organizado em uma arquitetura diferente, SDD4J pode ser uma opção interessante de escolha. 

No fim, são trade-offs diferentes.

E outros workflows de SDD resolvem outras partes do problema adotando restrições diferentes.

Isso é engenharia de software.

Flexibilidade não é automaticamente melhor do que restrição.

Mais automação não é automaticamente melhor do que julgamento humano.

Mais contexto não significa automaticamente contexto melhor.

E uma especificação não se torna automaticamente verdadeira simplesmente porque decidimos chamá-la de *source of truth*.

Começamos essa jornada perguntando como especificações poderiam conduzir melhor o desenvolvimento de software com agentes de código.

Experimentar essa pergunta nos levou a algo ligeiramente diferente.

Hoje, a pergunta que considero mais interessante é:

**Como podemos manter a intenção do software compreensível, verificável e próxima do comportamento que ela descreve enquanto o software continua evoluindo?**

É isso que queremos dizer com uma **Living Specification**.

SDD4J é o nosso experimento atual nessa direção, e convidamos você a experimentá-lo conosco.

O workflow se encontra no repositório [soujava/agent-skills](https://github.com/soujava/agent-skills), e você pode utilizá-lo em qualquer projeto Java. 

É open-source e gratuito, e estamos abertos a contribuições :smile:.

Acredito que ele vai evoluir, algumas de suas ideias podem se mostrar úteis e outras não. Mas, acima de tudo, espero que ele nos ajude a continuar em nossas explorações.

Até porque, nesse mundo onde agentes de código e modelos estão sempre mudando a forma como produzimos software, pode ser que o SDD4J se torne obsoleto e, estará tudo bem! Até porque workflows nunca deveriam ser artefatos permanentes, e sim continuar evoluindo!

**Workflows podem ser temporários, mas a intenção do software deve sobreviver a qualquer workflow.**

---

## E você, o que acha?

Este artigo representa o ponto em que nossos experimentos nos trouxeram até agora: **não uma resposta definitiva**.

E é justamente por isso que eu gostaria muito de conhecer a sua experiência.

Como você está utilizando especificações com agentes de código?

Você trata a especificação como *source of truth* ou também já encontrou *drift* entre especificações e o código que realmente está em produção?

Já experimentou Spec-Driven Development, SBCE, OpenSpec, Spec Kit ou alguma abordagem completamente diferente?

E que tal experimentar o SDD4J em um projeto Java existente? Ele pode ser útil para você, ou talvez não. Mas, de qualquer forma, espero que ele possa inspirar novas ideias.

E talvez a pergunta que mais me interessa:

**O que uma Living Specification significa para você?**

Compartilhe nos comentários suas experiências, discordâncias, experimentos e aprendizados. São justamente as diferentes experiências que tornam essa discussão valiosa.

E, se este artigo trouxe alguma ideia útil para você, **compartilhe com outro desenvolvedor ou time que também esteja experimentando desenvolvimento de software assistido por IA.**

Talvez a próxima ideia interessante comece justamente a partir dessa conversa.
