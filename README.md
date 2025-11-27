# Guia de estudos para o AWS Certified Developer – Associate (DVA-C02) 

![Tela inicial](./assets/img/aws-associate-developer.webp)

Este repositório foi criado para ajudar quem está se preparando para a certificação **AWS Certified Developer – Associate (DVA-C02)**. O foco é compartilhar **recursos gratuitos**, **explicações simples** e **dicas práticas** para quem está começando na nuvem AWS.

Este Conteúdo aborda apenas os temas principals do exame, para entender melhor o que é e como funcionam cada recurso, acesse o [Guia para o AWS Certified Cloud Practitioner (CLF-C02)](https://github.com/frontendclean/aws-practitioner-CLF-C02)

---

## Recursos de Estudo

### 1. **Guia do exame** 
- [AWS Guia do exame (português)](https://d1.awsstatic.com/pt_BR/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Exam-Guide.pdf)
  > Guia oficial da AWS.

- [Simula+ (Criado por @frontendclean) GRATUITO](https://simulaplus.com.br/)
  > Plataforma com diversas questões e simulados para praticar.

---

## O que estudar (+12 tópicos)

### Menu 

- [Padrões Arquitetônicos](#padrões-arquitetônicos)
- [Idempotência](#idempotência)
- [Diferenças entre os conceitos stateful e stateless](#diferenças-entre-os-conceitos-stateful-e-stateless)
- [Diferenças entre componentes com acoplamento rígido e acoplamento flexível ](#diferenças-entre-componentes-com-acoplamento-rígido-e-acoplamento-flexível)
- [Padrões de design tolerantes a falhas](#padrões-de-design-tolerantes-a-falhas)
- [Diferenças entre padrões síncronos e assíncronos](#diferenças-entre-padrões-síncronos-e-assíncronos)
- [Mapeamento de origem de eventos](#mapeamento-de-origem-de-eventos)
- [Chaves de partição de alta cardinalidade para acesso balanceado à partição](#chaves-de-partição-de-alta-cardinalidade-para-acesso-balanceado-à-partição)
- [Modelos de consistência de banco de dados](#modelos-de-consistência-de-banco-de-dados)
- [Diferenças entre as operações de consulta e varredura](#diferenças-entre-as-operações-de-consulta-e-varredura)
- [Chaves e indexação do Amazon DynamoDB](#chaves-e-indexação-do-amazon-dynamodb)
- [Estratégias de armazenamento em cache](#estratégias-de-armazenamento-em-cache)

---

### Padrões Arquitetônicos

Padrões arquitetônicos são conjuntos de princípios e templates de design que guiam a estrutura e a organização de um sistema de software. Eles definem as regras e os relacionamentos entre os componentes, garantindo escalabilidade, manutenibilidade e confiabilidade.

#### Arquitetura de Microsserviços (Microservices)

Este é, talvez, o padrão mais importante na computação em nuvem moderna.

##### O que é?

É uma abordagem onde uma aplicação grande é decomposta em um conjunto de serviços menores e independentes que se comunicam (geralmente via APIs leves, como HTTP/REST ou mensagens assíncronas). Cada serviço possui seu próprio código, stack de tecnologia e, muitas vezes, seu próprio banco de dados.

##### Vantagens

  - Escalabilidade Independente: Você pode escalar apenas o serviço que está sob alta demanda, economizando recursos.
  - Resiliência: A falha em um serviço geralmente não derruba a aplicação inteira.
  - Desenvolvimento Ágil: Equipes menores podem desenvolver, implantar e atualizar serviços de forma independente e rápida.

##### Analogia

Pense em uma lanchonete fast-food (o monolito tradicional) onde um único cozinheiro faz tudo (pedir, cozinhar, limpar). Mudar um ingrediente afeta toda a operação. A Arquitetura de Microsserviços é como uma cozinha industrial onde há estações separadas: uma para bebidas, outra para sanduíches, e outra para batatas fritas, cada uma operada por uma equipe especializada. Se o setor de sanduíches ficar ocupado, você contrata mais gente somente para essa estação.

##### Serviços AWS Relevantes

Amazon ECS, Amazon EKS (para orquestração de containers), AWS Lambda (para serverless), Amazon API Gateway e AWS App Mesh.

#### Arquitetura Orientada a Eventos (Event-Driven Architecture - EDA)

Essencial para sistemas que precisam reagir em tempo real.

##### O que é?

Um padrão onde os componentes do sistema se comunicam por meio da produção, detecção, consumo e reação a eventos. Um componente (Produtor) emite um evento (uma notificação de que algo aconteceu, ex: "Pedido Criado"), e outros componentes (Consumidores) que estão interessados naquele tipo de evento o recebem e reagem a ele, sem saber quem o produziu.

##### Vantagens

  - Acoplamento Flexível (Loose Coupling): O produtor e o consumidor não dependem um do outro. O produtor nem sabe quem são os consumidores.
  - Distribuição Assíncrona: Permite que as tarefas demoradas sejam processadas em segundo plano, melhorando o desempenho da interface do usuário.
  - Escalabilidade: Aumenta a capacidade de lidar com picos de tráfego.

##### Analogia

É como um jornal mural em um escritório. Em vez de ligar para cada pessoa (conexão direta) para dizer que "o café acabou", você simplesmente escreve um aviso no mural (o evento). Qualquer um que olhar o mural (o listener) e se importar (o setor de compras, por exemplo) pode reagir (pedir mais café).

##### Serviços AWS Relevantes

Amazon SQS (Fila Simples), Amazon SNS (Serviço de Notificação), Amazon Kinesis (Streaming de dados) e o Amazon EventBridge (o principal hub de eventos serverless da AWS).

#### Arquitetura Serverless

Foco na lógica de negócios, não na infraestrutura.

##### O que é?

Um modelo de desenvolvimento onde o desenvolvedor não se preocupa com a manutenção, provisionamento ou escalabilidade dos servidores. O provedor de nuvem (AWS) gerencia toda a infraestrutura subjacente. Você paga apenas pelo que usa (Pay-per-use).

##### Vantagens

  - Zero Gerenciamento de Servidores: Você se concentra apenas no código.
  - Escalabilidade Automática: A AWS escala a infraestrutura de forma automática e instantânea para lidar com qualquer volume de tráfego.
  - Custo-benefício: Ideal para cargas de trabalho intermitentes, pois você não paga pelo tempo ocioso.

##### Analogia

É como a água encanada em sua casa. Você não precisa se preocupar em construir e manter um reservatório ou sistema de bombeamento (o servidor). Você simplesmente liga a torneira (executa seu código) e paga pela quantidade de água que consumiu.

##### Serviços AWS Relevantes

AWS Lambda (o coração do serverless), Amazon DynamoDB (banco de dados NoSQL serverless), Amazon S3 e Amazon API Gateway.

---

### Idempotência

A Idempotência é uma propriedade crítica em sistemas distribuídos, especialmente em ambientes de nuvem como a AWS, onde a falha de rede e a repetição de solicitações (retries) são comuns.

##### O que é?

Idempotência é a propriedade de uma operação que garante que ela pode ser aplicada múltiplas vezes sem alterar o resultado além da aplicação inicial. Em outras palavras, executar a ação uma vez ou mil vezes tem o mesmo efeito final no estado do sistema.

##### Vantagens

  - Confiabilidade em Redes: A principal razão. Se um cliente envia uma requisição e não recebe uma resposta (devido a um timeout ou falha de rede), ele pode tentar novamente. Sem idempotência, essa segunda tentativa duplicaria a ação (ex: cobraria o cliente duas vezes). Com idempotência, a segunda requisição é reconhecida como uma repetição e a ação original não é reexecutada.
  - Consistência de Dados: Evita a criação de recursos duplicados, lançamentos contábeis duplicados ou alterações de estado inconsistentes.
  - Facilidade de Retry: Simplifica o lado do cliente, pois ele pode repetir a requisição com segurança até receber uma resposta.

##### Analogia

Imagine um botão de pagamento em um aplicativo de e-commerce.

Não Idempotente: Se você clicar no botão uma vez e a rede falhar (o pagamento foi processado, mas você não viu a confirmação) e você clicar novamente, você será cobrado duas vezes.

Idempotente: O aplicativo gera uma Chave de Idempotência única (como um código UUID) e a envia com o primeiro clique. Se você clicar novamente, o sistema recebe a mesma chave. Ele verifica: "Já vi esta chave? Sim. Então, em vez de processar o pagamento novamente, vou apenas retornar o status de sucesso do primeiro processamento." O resultado final é que o cliente foi cobrado exatamente uma vez.

---

### Diferenças entre os conceitos stateful e stateless

#### Stateless (Sem Estado)

No modelo Stateless, o servidor não tem "memória" de interações passadas. Para que uma requisição seja processada, ela deve carregar toda a informação necessária (ex: token de autenticação, dados da transação).

##### Vantagens

  - Escalabilidade Ilimitada: Um Load Balancer (ALB/NLB) pode distribuir o tráfego para qualquer instância EC2 ou função Lambda, garantindo que o sistema possa lidar com milhões de usuários.
  - Simplicidade: Não é necessário gerenciar a sincronização do estado entre servidores.
  - Alta Disponibilidade/Resiliência: Se um servidor falhar, o cliente pode simplesmente tentar novamente, e outro servidor processará a requisição sem problemas.

##### Analogia

É como fazer uma compra em uma máquina de vendas automática. Você insere a moeda e seleciona o produto. A máquina não "lembra" de você da última vez. Cada transação é completa e isolada.


##### Implementação na AWS

  - Aplicações Web (EC2 ou Container) que usam Amazon DynamoDB ou Amazon ElastiCache para armazenar o estado da sessão fora do servidor de aplicação.
  - Funções AWS Lambda, que são inerentemente stateless e escalam infinitamente.
  - Security Groups são stateful (veja abaixo), mas as Network ACLs são stateless (exigem regras de entrada e saída explícitas).

#### Stateful (Com Estado)

Em um sistema Stateful, o servidor mantém o estado da sessão de um usuário.
É essencial para componentes que precisam gerenciar o fluxo de dados em um determinado momento ou persistir dados.

  - Bancos de Dados (RDS, DynamoDB): Precisam ser stateful para garantir que os dados escritos permaneçam lá.
  - Servidores de Cache: Armazenam dados em memória.
  - Serviços de Fila (SQS, Kinesis): O estado da fila (quais mensagens foram enviadas, recebidas ou processadas) é mantido.
  - Grupos de Segurança (Security Groups) da AWS: São stateful. Se você permite o tráfego de entrada em uma porta, o tráfego de retorno (resposta) é automaticamente permitido, sem a necessidade de uma regra de saída explícita.

##### Analogia

É como uma conversa por telefone. As informações trocadas anteriormente afetam o que você diz em seguida. Se a ligação cair, você precisa ligar de volta e (geralmente) retomar a conversa de onde parou.

##### Desafio na AWS

A maneira de obter a escalabilidade horizontal para um componente stateful é externalizar o estado e usar soluções de persistência e replicação de dados. Por isso, a arquitetura moderna da AWS se concentra em ter aplicações stateless que se comunicam com serviços de dados stateful gerenciados pela própria AWS.

---

### Diferenças entre componentes com acoplamento rígido e acoplamento flexível 

#### Acoplamento Rígido (Tight Coupling)

Ocorre quando dois ou mais componentes são fortemente dependentes um do outro. Uma mudança, falha ou indisponibilidade em um componente impacta diretamente o outro.

##### O que é?

Os componentes (Módulo A e Módulo B) conhecem e dependem fortemente dos detalhes internos um do outro (como o nome exato de uma classe, a estrutura de dados interna ou a implementação de uma função).

##### Vantagens

  - Simplicidade: Em sistemas muito pequenos e monolíticos, pode ser mais fácil de implementar inicialmente.
  - Desempenho (Teórico): Comunicação em memória (dentro de um único processo) é extremamente rápida.

##### Desvantagens

  - Falta de Resiliência: Se o Módulo B cair, o Módulo A não consegue funcionar e falha.
  - Dificuldade de Manutenção: Uma pequena mudança no Módulo B exige que o Módulo A também seja modificado, testado e reimplantado.
  - Escalabilidade Limitada: Os componentes precisam ser escalados juntos.

##### Exemplo

Um monolito que chama diretamente uma função (método) em um outro módulo, e ambos compartilham o mesmo banco de dados. Se a função chamada falhar, toda a transação falha.

#### Acoplamento Flexível (Loose Coupling)

Ocorre quando os componentes são independentes e se comunicam através de interfaces bem definidas, sem conhecer os detalhes de implementação um do outro.

##### O que é?

Os componentes interagem por meio de um contrato (como uma API, um formato de mensagem ou um evento). O Módulo A envia uma mensagem e não se importa como o Módulo B (ou C, D, etc.) vai processá-la.

##### Vantagens

  - Resiliência (Fault Isolation): Se o Módulo B falhar, o Módulo A pode continuar funcionando (por exemplo, a mensagem fica na fila até B voltar). A falha é isolada.
  - Escalabilidade Independente: Você pode escalar o Módulo A e o Módulo B de forma totalmente separada, otimizando custos e recursos.
  - Flexibilidade e Manutenção: O Módulo B pode ser reescrito em outra linguagem ou ter sua lógica alterada sem que o Módulo A precise de qualquer alteração, desde que o contrato de comunicação seja mantido.

##### Exemplo

Arquitetura Orientada a Eventos: O Módulo A (Serviço de Pedidos) publica um evento: "Pedido Criado" no Amazon SNS ou EventBridge. O Módulo B (Serviço de Estoque) e o Módulo C (Serviço de Faturamento) estão inscritos nesse evento. O Serviço de Pedidos não sabe quem são eles e nem se importa se eles estão no ar naquele momento.

---

### Padrões de design tolerantes a falhas

Na AWS, dois padrões são cruciais para a tolerância a falhas: Novas Tentativas (Retries) com Backoff Exponencial e Jitter e Filas de Mensagens Mortas (Dead Letter Queues - DLQ).

#### Novas Tentativas (Retries) com Backoff Exponencial e Jitter

Este padrão é usado quando um serviço (Cliente) tenta acessar outro serviço (Servidor), mas a chamada falha devido a problemas temporários (como throttling, congestionamento de rede ou falhas transitórias).

##### O que é?

É uma técnica para repetir chamadas com falha de forma inteligente, dando ao serviço que está sendo chamado tempo para se recuperar e evitando sobrecarregá-lo ainda mais.

###### Novas Tentativas (Retries)

O cliente repete a solicitação após uma falha.

###### Backoff Exponencial

A repetição da chamada não é feita imediatamente. O tempo de espera entre as tentativas aumenta exponencialmente (ex: 1s, 2s, 4s, 8s...). Isso dá ao serviço tempo para se recuperar.

###### Jitter (Aleatoriedade)

É a adição de um pequeno atraso aleatório ao tempo de backoff.

Por que o Jitter? Se milhares de clientes falharem ao mesmo tempo, e todos usarem o mesmo backoff (ex: 4s), eles tentarão novamente exatamente no mesmo instante, criando um novo pico de tráfego (conhecido como Thundering Herd Problem). O jitter espalha essas novas tentativas no tempo, suavizando a carga no servidor.

##### Implementação na AWS

Muitos SDKs e serviços da AWS (como o SDK para S3, DynamoDB, Lambda) já implementam automaticamente esse padrão. Em suas funções Lambda, você pode usar bibliotecas como AWS Lambda Powertools para aplicar facilmente o Backoff Exponencial e Jitter em chamadas a serviços externos.

#### Filas de Mensagens Mortas (Dead Letter Queues - DLQ)

Este padrão é usado em arquiteturas orientadas a eventos e com filas (SQS, SNS com endpoints), onde a falha não é temporária, mas persistente.

##### O que é?

Uma DLQ é uma fila SQS separada que armazena mensagens que não puderam ser processadas com sucesso após um número máximo definido de tentativas.

##### Vantagens

  - Isolamento de Falhas: Remove mensagens "venenosas" (poison pills) da fila principal. Uma mensagem que causa falha repetidamente poderia travar toda a fila (fazendo com que os consumidores gastassem todas as tentativas nela). A DLQ isola o problema.
  - Análise e Debug: O desenvolvedor pode inspecionar as mensagens na DLQ para entender a causa da falha (ex: formato de dados inválido, bug no código do consumidor, dependência indisponível).
  - Prevenção de Perda de Dados: Garante que mensagens valiosas, mesmo que falhem no processamento, não sejam descartadas.

#####  Como Funciona

  - Você tem uma Fila Principal SQS (ou um endpoint Lambda, SNS, etc.).
  - Você configura um Redrive Policy na Fila Principal, especificando o maxReceiveCount (o número máximo de vezes que a mensagem pode ser tentada).
  - Você especifica o DLQ de destino.
  - Se uma mensagem for recebida e falhar (não for excluída) o número de vezes estipulado (maxReceiveCount), o SQS/serviço a move automaticamente para a DLQ.

##### Analogia

Pense em uma esteira de produção (a fila principal). Se uma peça for defeituosa e o robô tentar repará-la várias vezes sem sucesso, a esteira não pode parar. A peça defeituosa é desviada para uma caixa de descarte/inspeção (a DLQ) para análise posterior, permitindo que a produção continue com as peças boas.

##### Serviços AWS Relevantes

  - Amazon SQS: O serviço primário para filas e DLQs.
  - AWS Lambda: Pode ser configurado para usar uma DLQ (SNS ou SQS) para mensagens que falham no processamento assíncrono.
  - Amazon SNS: Também pode usar DLQs para endpoints que falham ao entregar notificações (ex: um endpoint Lambda que falha).

---

### Diferenças entre padrões síncronos e assíncronos

#### Padrão Síncrono (Synchronous)

No padrão síncrono, a comunicação é realizada em tempo real e de forma bloqueante.

##### O que é?

O componente que inicia a ação (Cliente) deve esperar pela resposta imediata do componente chamado (Servidor) antes de prosseguir com a próxima etapa. O fluxo de execução fica "bloqueado" até que a resposta chegue.

##### Vantagens

  - Simples de Implementar: O fluxo é direto e fácil de seguir (request-response).
  - Feedback Imediato: O cliente recebe o resultado (sucesso ou falha) da operação instantaneamente.

##### Desvantagens

  - Bloqueio: Se o serviço chamado demorar ou falhar, o cliente fica parado, consumindo recursos e aumentando a latência.
  - Acoplamento Rígido: O cliente e o servidor precisam estar disponíveis ao mesmo tempo. Se um cair, o outro falha.
  - Escalabilidade Limitada: Requer mais recursos, pois o cliente (ex: uma função Lambda) precisa ficar ativo durante todo o tempo de espera.

##### Serviços AWS Relevantes 

Amazon API Gateway, Application Load Balancer (ALB), e chamadas diretas de serviço para serviço (ex: Lambda para Lambda via RequestResponse invocation type).

#### Padrão Assíncrono (Asynchronous)

No padrão assíncrono, a comunicação é realizada sem esperar por uma resposta imediata, sendo ideal para a maioria das operações na nuvem.

##### O que é?

O componente que inicia a ação (Produtor) envia uma mensagem ou evento para um intermediário (uma fila ou tópico) e imediatamente continua sua execução (não-bloqueante). O componente que processa a ação (Consumidor) retira a mensagem do intermediário e a processa quando está pronto.

##### Vantagens

  - Resiliência (Tolerância a Falhas): Se o Consumidor estiver indisponível, o intermediário (fila) armazena a mensagem, prevenindo a perda de dados.
  - Escalabilidade (Desacoplamento): O Produtor e o Consumidor operam independentemente e podem ser escalados separadamente.
  - Maior Desempenho: O Produtor não espera, liberando recursos mais rapidamente.

##### Desvantagens

  - Complexidade: O fluxo de ponta a ponta é mais difícil de rastrear.
  - Feedback Atrasado: O Cliente não recebe a resposta final da operação de imediato (o resultado deve ser enviado por outro canal, como um callback ou notificação).

##### Serviços AWS Relevantes 

Amazon SQS (Filas), Amazon SNS (Tópicos), Amazon EventBridge (Eventos), AWS Lambda (invocação assíncrona), Amazon Kinesis (Streams de dados).

---

### Mapeamento de origem de eventos

O Mapeamento de Origem de Eventos (Event Source Mapping - ESM) é um conceito crucial no AWS Lambda. Ele define como o Lambda consome itens de streams ou filas de outros serviços da AWS.

O Mapeamento de Origem de Eventos é o que permite que uma função Lambda processe automaticamente dados de serviços como Amazon DynamoDB Streams, Amazon Kinesis, Amazon SQS e Apache Kafka Gerenciado pela Amazon (MSK).

Ao invés de você ter que escrever um código que periodicamente verifica uma fila SQS ou um stream Kinesis (o que seria polling manual), o AWS Lambda gerencia isso para você.

O ESM é um recurso do lado do Lambda que atua como um agente de polling dedicado. Ele lê dados da origem do evento e invoca sua função Lambda de forma assíncrona, passando o lote de dados como o parâmetro event.

---

### Chaves de partição de alta cardinalidade para acesso balanceado à partição

#### Chaves de Partição (Partition Keys)

Em bancos de dados distribuídos (como o DynamoDB), o armazenamento de dados é feito em vários servidores ou partições. A Chave de Partição é o atributo principal usado para determinar em qual partição física um item será armazenado.

Alto Desempenho = Partições Balanceadas

O objetivo principal do uso de boas chaves de partição é garantir o acesso balanceado à partição.

  - Se as solicitações de leitura e gravação forem distribuídas uniformemente por todas as partições disponíveis, você consegue máximo throughput (vazão) e baixa latência.
  - Se muitas solicitações se concentrarem em apenas uma partição, ela se torna um "hot partition" (partição quente), limitando o desempenho geral do sistema e causando erros de throttling.

#### Chaves de Partição de Alta Cardinalidade

Cardinalidade refere-se ao número de valores únicos em uma coluna ou atributo.

  - Chave de Alta Cardinalidade: Um atributo que possui um número muito grande de valores únicos em relação ao número total de itens (ex: um ID de usuário único, um GUID/UUID).

##### Vantagens

  - Distribuição Uniforme: O DynamoDB (ou outro sistema distribuído) pode usar o grande número de valores únicos para espalhar os itens da sua tabela de maneira muito mais uniforme por um número maior de partições físicas.
  - Prevenção de Hot Partitions: Como cada ID de usuário (por exemplo) vai para uma partição diferente, o tráfego de usuários é automaticamente distribuído, impedindo que qualquer partição única seja sobrecarregada.
  - Throughput Máximo: Ao distribuir a carga, você pode utilizar todo o throughput provisionado ou sob demanda da sua tabela.

##### Analogia

O Estacionamento

  - Baixa Cardinalidade (Ruim): Imagine um estacionamento (o banco de dados) dividido em 10 andares (as partições). Se você decidir que a "Chave de Partição" é a cor do carro (Baixa Cardinalidade: Vermelho, Azul, Preto, etc.). Todos os carros Pretos iriam para o andar 1, todos os Azuis para o andar 2, etc. Se 80% dos carros forem Pretos, o Andar 1 se torna uma "hot partition", ficando congestionado e lento, enquanto outros andares ficam vazios.

  - Alta Cardinalidade (Bom): Se a "Chave de Partição" for a Placa do Carro (Alta Cardinalidade: milhões de valores únicos). O sistema usa a placa para espalhar os carros aleatoriamente por todos os 10 andares. O tráfego de entrada e saída é distribuído uniformemente, e o estacionamento opera com eficiência máxima.

##### O Problema das Chaves de Baixa Cardinalidade

Usar uma chave de partição de Baixa Cardinalidade (poucos valores únicos, ex: status='ativo', região='us-east-1') é um erro comum que leva a problemas de desempenho, pois o grande volume de dados será agrupado em poucas partições, limitando a escalabilidade.

---

### Modelos de consistência de banco de dados

#### Consistência Forte (Strong Consistency)

A consistência forte é o modelo tradicional e mais rigoroso, geralmente associado a bancos de dados relacionais (SQL).

##### O que é?

O sistema garante que, após uma operação de gravação bem-sucedida, qualquer leitura subsequente retornará o valor atualizado, mesmo que a leitura seja feita em uma réplica ou nó diferente.

##### Vantagens

  - Simples para o desenvolvedor, pois não há necessidade de lidar com dados defasados. Essencial para transações críticas (ex: débitos bancários).

##### Desvantagens

  - Latência Aumentada: Exige que o sistema espere pela confirmação de que todos os nós/réplicas relevantes foram atualizados antes de retornar o sucesso ao gravador.
  - Escalabilidade Limitada: Pode restringir a escalabilidade horizontal, pois a coordenação entre os nós é mais complexa.

##### Serviços AWS Relevantes 

  - Amazon RDS: Bancos de dados relacionais (PostgreSQL, MySQL, etc.) geralmente fornecem consistência forte (modelo ACID).
  - Amazon DynamoDB: Oferece consistência forte como uma opção, geralmente para leituras diretas na Tabela Base.

#### Consistência Eventual

A consistência eventual é o modelo mais comum e otimizado para desempenho em sistemas distribuídos de larga escala (NoSQL).

##### O que é?

O sistema retorna o sucesso da gravação imediatamente, mas a atualização é propagada de forma assíncrona para todas as réplicas. Existe uma janela de tempo (geralmente milissegundos) onde as leituras podem retornar dados antigos (stale data).

##### Vantagens

  - Baixa Latência: A escrita é muito rápida, pois não precisa esperar pela confirmação de todas as réplicas.
  - Alta Disponibilidade e Escalabilidade: Ideal para sistemas de tráfego intenso (como redes sociais ou e-commerce) onde é mais importante estar sempre online do que garantir dados totalmente frescos.

##### Desvantagens

  - O desenvolvedor precisa considerar cenários onde leituras rápidas podem retornar dados desatualizados.

##### Serviços AWS Relevantes 

  - Amazon S3: É eventualmente consistente para novas escritas (PUT).
  - Amazon DynamoDB: É o padrão padrão para leituras (é mais rápido e consome menos capacidade).
  - Índices Secundários Globais (GSIs) do DynamoDB: São eventualmente consistentes em relação à Tabela Base.

---

### Diferenças entre as operações de consulta e varredura

#### Consulta (Query)

A operação de Consulta é a maneira mais eficiente e preferida de recuperar dados. Ela é altamente performática porque acessa os dados diretamente com base na chave de partição.

##### O que faz?

Busca itens com base em um valor de Chave de Partição específico e, opcionalmente, em um filtro na Chave de Classificação (Sort Key).

##### Como funciona

O DynamoDB (ou outro sistema) usa o valor da Chave de Partição fornecido para direcionar a busca para o conjunto específico de partições (ou o shard) onde os dados estão armazenados.

##### Eficiência

Alta. A operação é focada e consome apenas recursos relacionados à partição específica.

##### Restrições

Requer que você forneça o valor exato da Chave de Partição da tabela ou índice.

##### Analogia

É como procurar um livro em uma biblioteca onde você conhece o corredor (Chave de Partição) e o número da estante (Chave de Classificação). Você vai direto ao ponto.

#### Varredura (Scan)

A operação de Varredura é a maneira menos eficiente de recuperar dados e deve ser evitada em ambientes de produção de alto tráfego, a menos que seja estritamente necessário.

##### O que faz?

Lê todos os itens da tabela ou índice especificado. Opcionalmente, pode-se aplicar uma condição de filtro no lado do cliente para descartar alguns dados antes de retorná-los.

##### Como funciona

O DynamoDB lê todos os dados em todas as partições da tabela.

##### Eficiência

Baixa.

  - Custo/Tempo: Geralmente, uma operação de Scan leva muito mais tempo e consome muito mais Capacidade de Leitura (RCU) do que uma Query equivalente, pois lê todos os dados.
  - Throttling: O Scan pode consumir toda a capacidade de leitura da tabela rapidamente, causando throttling para outras operações.

##### Analogia

É como procurar um livro em uma biblioteca olhando em cada estante, em cada corredor, até encontrá-lo, mesmo sabendo o nome.

---

### Chaves e indexação do Amazon DynamoDB

O DynamoDB é um banco de dados NoSQL de valor-chave e documento, e suas chaves são a principal forma de acessá-lo.

#### Chaves Primárias (Primary Keys)

Toda tabela no DynamoDB deve ter uma Chave Primária, que identifica exclusivamente cada item na tabela. Existem dois tipos de Chave Primária:

##### Chave de Partição Simples (Partition Key)

Um único atributo (coluna) que serve como Chave Primária. O DynamoDB usa o valor desta chave como input para uma função de hashing para determinar a partição física onde o item será armazenado.

##### Chave Composta

Combinação de dois atributos: uma Chave de Partição e uma Chave de Classificação (Sort Key).

#### Indexação (Acessos Alternativos)

A Chave Primária só permite consultar os dados de uma maneira. Para oferecer maneiras alternativas de acessar os dados, o DynamoDB usa Índices.

##### Índice Secundário Global (Global Secondary Index - GSI)

Um índice com uma Chave Primária totalmente diferente da Chave Primária da tabela principal. O GSI é, efetivamente, uma nova tabela, mas é populada automaticamente e de forma assíncrona (eventualmente consistente) pelo DynamoDB.

Essencial para suportar padrões de consulta que não usam a Chave Primária original.

Exemplo: A tabela principal é indexada por UsuarioID. Você cria um GSI indexado por Email para permitir a busca de usuários por e-mail.

##### Índice Secundário Local (Local Secondary Index - LSI)

Um índice que compartilha a mesma Chave de Partição da tabela principal, mas possui uma Chave de Classificação diferente.

Permite classificar os dados dentro de uma partição existente de uma maneira diferente.

Exemplo: Tabela com PK=UsuarioID e SK=DataPedido. Você pode criar um LSI com PK=UsuarioID e SK=ValorPedido para buscar pedidos de um usuário por valor, em vez de por data.

---

### Estratégias de armazenamento em cache

As Estratégias de Armazenamento em Cache (Caching Strategies) são fundamentais para reduzir a latência, diminuir a carga sobre o banco de dados principal e, consequentemente, reduzir custos

#### Tipos de Estratégias

As estratégias de cache definem como o cache interage com a aplicação e com o banco de dados persistente (datastore).

##### Read-Through (Leitura Direta)

O cache é posicionado entre a aplicação e o banco de dados.

Leitura: A aplicação sempre consulta o cache primeiro.

  - Se os dados estiverem no cache (Cache Hit), eles são retornados imediatamente.
  - Se os dados não estiverem no cache (Cache Miss), o próprio cache é responsável por buscar os dados no banco de dados principal, armazená-los e, então, retorná-los à aplicação.

###### Vantagens

A lógica de carregamento de dados é gerenciada pelo cache, simplificando o código da aplicação.

##### Lazy Loading (Carregamento Preguiçoso)

Semelhante ao Read-Through, mas a lógica de carregamento é tipicamente implementada pela aplicação.

Leitura: A aplicação verifica o cache.

  - Cache Hit: Retorna os dados.
  - Cache Miss: A aplicação busca os dados no banco de dados, escreve-os no cache (populating the cache) e, então, retorna os dados.

###### Vantagens

Somente os dados solicitados são carregados no cache (ótimo para dados esparsos), economizando memória e evitando o carregamento de dados nunca usados.

###### Desvantagens

Maior latência no primeiro acesso (Cache Miss), pois requer duas chamadas (leitura do cache e leitura do DB).

##### Write-Through (Escrita Direta)

A aplicação escreve dados simultaneamente no cache e no banco de dados.

Escrita: A aplicação atualiza o cache e o banco de dados dentro da mesma transação.

###### Vantagens

Garante que o cache e o banco de dados estejam sempre sincronizados (consistência forte na escrita).

###### Desvantagens

Aumenta a latência da escrita, pois a aplicação deve esperar que tanto o cache quanto o banco de dados persistente confirmem a operação.

##### Write-Back (Escrita de Retorno)

A aplicação escreve apenas no cache e recebe a confirmação imediata. O cache transfere a escrita para o banco de dados assincronamente em segundo plano.

###### Vantagens

Escritas extremamente rápidas.

###### Desvantagens

Risco de perda de dados se o cache falhar antes de a escrita ser persistida no banco de dados (baixa durabilidade).


#### TTL (Time-To-Live)

O TTL (Time-To-Live) é o mecanismo essencial para gerenciar a validade dos dados no cache e é usado em praticamente todas as estratégias.

Um valor numérico (em segundos) anexado a um item no cache, que especifica por quanto tempo o item é considerado válido.

Quando o tempo do TTL expira, o item é automaticamente removido do cache ou marcado como inválido.

O TTL é a forma mais simples de resolver o problema de coerência do cache (garantir que os dados do cache não sejam muito antigos).

  - Se os dados no banco de dados mudam frequentemente, você usa um TTL curto.
  - Se os dados mudam raramente (ou são estáticos), você usa um TTL longo.

---

## Dicas Finais

- Revise os conceitos, não decore.
- Faça simulados com foco em entender o **porquê da resposta**.
- Use analogias do dia a dia para entender serviços.
- Mantenha a calma no exame — é introdutório!

---

## Contribua

Achou um recurso legal? Melhoraria algum trecho? Sinta-se à vontade para abrir um PR ou criar uma issue!

---

## Contato

Caso tenha dúvidas ou queira trocar ideias, me chama no [Instagram](https://www.instagram.com/frontend_clean/).

---

**Bons estudos e boa sorte na prova!**