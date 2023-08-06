# Section 4 - The Architecture Process

Processo
: - entender os requisitos do sistema (O que o sistema faz)
  - entender os requisitos não funcionais
  - mapear os componentes (Representa as tarefas do sistema)
  - selecionar a stack (Selecionar as tecnologias que serão usadas no backend, no frontend e no banco de dados)
  - realizar o design da arquitetura
  - escrever a documentação da arquitetura (A documentação deve ser útil para o CEO, CIO, project manager, team leader e os desenvolvedores)
  - dar suporte ao time

# Section 5 - Working with System Requirements
Requisitos funcionais (mais fáceis de identificar - o que o sistema faz) e não funcionais. Não ignorar os requisitos funcionais. Nunca comece o sistema sem considerar os requisitos não funcionais. Ao perguntar sobre requisitos não funcionais o arquiteto de software deve trazer informações mais realistas e com números para o cliente para chegar em um bom resultado.

## Principais requisitos não funcionais
- Performance (Quantas requisições ao mesmo tempo uma API aguenta.)
- Carregamento de dados (Qual o tamanho da requisição em MB, ao invés de usar REST usou outra arquitetura quando precisou processar um dado de 700 MB.)
- Volume de dados (Quanto de disco será utilizado pela aplicação ao longo do tempo.)
- Usuários utilizando o sistema de maneira concorrente (ao mesmo tempo)
- SLA ou Service Level Agreement (Tempo obrigatório em que a aplicação não deve sofrer queda [downtime])

## Notas
[Deploy blue green](https://www.redhat.com/en/topics/devops/what-is-blue-green-deployment)

# Section 6 - Application Types

### Principais tipos de aplicações
- web apps (utilizados quando precisa de uma UI, quando vários usuários irão utilizar, sistema baseado em request/response)
- web API (geralmente usa REST, URL, Parâmetros de url, verbos http, retorna dados ao invés de HTML, baseado em request/response)
- mobile (recomendado para aplicações que requerem interação do usuário, lida com localização)
- console (não precisa de UI, interação limitada, o processamento de dados pode ser longo ou curto)
- service (parecido com a aplicação de console, porém é gerenciado pelo Service Manager do sistema operacional)
- desktop (muito utilizado em jogos)

### Outros tipos de aplicações
- function as a service (AWS Lambda, Azure Functions)

# Section 7 - Selecting Technology Stack
Há escolha da stack é irreversível. A decisão deve ser tomada de forma que haja uma documentação e seja um esforço em grupo. Levar em consideração o tamanho da comunidade da stack e também o quão ativa ela é (Utilizar o Stack Overflow). Considerar a popularidade da tecnologia no Google Trends.

A plataforma deve atender aos requisitos funcionais. (Ela deve ser capaz de realizar o que a aplicação necessita).

Tipos de banco de dados. SQL x NoSQL. Sql utiliza o ACID (Atomicity, Consistency, Isolation, Durability). NoSql foca em escalabilidade e performance.

SQL
: Para sistemas que usam poucos dados e que a consistência dos dados é importante

NoSql
: Lida com grandes quantidades de dados (10 terabytes+)

# Section 8 - Meet the *-ilities

- Escalabilidade (Adicionar recursos computacionais sem qualquer interrupção) (Escalabilidade vertical e horizontal [possui redundância, não tem limites de VM's])
- Manutenibilidade (Quem reporta os problemas na aplicação [Os usuários ou a máquina que está rodando a aplicação])
- Modularidade
- Extensibilidade (Extender código ao invés de ter que mudar o código original [Open/Closed Principle])
- Testabilidade (Manual, Unitário, Integração [Testa um processamento de vários passos])

# Section 9 - Component's Architecture
- Camadas (UI / Business / DAL). Camadas != Tiers.
- Interfaces (Deixa o código pouco acoplado [new significa acoplamento.])
- DI (Dependency Injection)
- SOLID (Single Responsibility Principle / Open Closed Principle / Liskov Substitution Principle / Interface Segregation Principle / Dependency Inversion Principle)
- Naming Conventions (CamelCase / Hungarian Notation [Evitar])
- Lidar com exceções (Sempre lide com exceções específicas / Sempre use try catch somente no código que pode gerar exceção)
- Logging (Usar logs para rastrear erros [Kibana])

# Section 10 - Design Patterns 101
Padrões de projeto ajudam o sistema a ser mais confiável e manutenível o que influencia a arquitetura do sistema.

- Factory (Criar a classe sem especificar a classe exata do objeto)
- Repository
- Façade 
  : Example:
  ```cs 
  class MoneyTransfer {
    public bool CheckAccountExist(int account)...
    public bool HasEnoughMoney(int account, double sum)...
    public void WithdrawMoney(int account, double sum)...
    public void DepositMoney(int account, double sum)...
    public void WriteLog(int account, string msg)...

    public bool TransferMoney(int accountFrom, int accountTo, double sum) 
    {
      if(!CheckAccountExist(accountFrom) || !CheckAccountExist(accountTo) || !HasEnoughMoney(accountFrom, sum))
      {
        return false;
      }

      WithdrawMoney(accountFrom, sum);

      DepositMoney(accountTo, sum);

      WriteLog(accountFrom, "Money Transferred.");
      WriteLog(accountTo, "Money Transferred.");

      return true;
  } 
  ```
- Command

# Section 11 - System Architecture
- Baixo acoplamento
- Stateless
- Cache (In-Memory, In-Process Cache / Distributed Cache)
- Messaging (Performance / Tamanho da mensagem / Tempo de execução d processamento / Feedback / Complexidade) [REST / WebSockets / Mensageria / File - DB based]
- Logging e Monitoramento (Logstash)

# Section 12 - External Considerations
Deadlines
: Douglas Adams The Hitchhiker's Guide to the Galaxy

Skills do Time
: Utilizar tecnologias desconhecidas pelo time pode ocorrer atraso na entrega, qualidade baixa

Suporte da equipe de TI
: Verificar se a equipe de TI da suporte para as ferramentas que serão utilizadas na aplicação (NoSQL databases, Message Broker)

Custo

# Section 13 - Architecture Document
O desenvolvimento da aplicação não deve começar antes de produzir o documento da arquitetura

Pessoas que irão ler a documentação da arquitetura
- Project Manager
- CTO
- QA Lead
- Desenvolvedores

Formato da documentação
: UML. Sempre busque uma arquitetura que seja simples

O documento deve informar o papel do sistema a partir de uma visão de negócios, se for migrar um sistema antigo as razões para a migração, por exemplo as tecnologias do projeto antigo estão desatualizadas e não há mais suporte para elas, e informar o impacto da aplicação.
O documento deve definir os requisitos da aplicação.
O documento terá um resumo em termos executivos de forma que pessoas que não tenham conhecimentos técnicos conseguirão saber o que o sistema faz.
Esse documento também terá um overview onde deverá ser descritos informações gerais da arquitetura para o time de desenvolvedores, diagramas da arquitetura, incluindo diagramas de sequência.
Por fim terá uma seção descrevendo os componentes da arquitetura.

Exemplos de requisitos:
- O sistema terá 150 usuários e o é esperado que haverá 20 usuários usando a aplicação ao mesmo tempo
- No primeiro dia a aplicação terá 100 GB de dados migrados do antigo sistema e crescerá cerca de 400 GB por ano
- SLA: O sistema ficará fora do ar cerca de 5 dias no ano, considerando interrupções esperadas e não esperadas

# Section 14 - Case Study

### IOToo

Requisitos
- Quantas mensagens ao mesmo tempo provavelmente serão recebidas em um momento de pico?
- Qual o total de mensagens por mês?
- Em média qual o tamanho da mensagem?
- Qual o tempo máximo de downtime permitido? (SLA Level -> Silver, Gold, Platinum)

Componentes
- Receiver
- Handler
- Info Provider

Comunicação
- REST API para os dispositivos IOT comunicar com o Receiver
- Queue para a API comunicar com o Handler (este acessa o banco de dados)
- O Info Provider usa uma REST API e consome o mesmo banco que o Handler
- O Handler comunica com uma Queue para fazer o Logging

Notas:
: Query Params devem ser atributos que não representam entidades e Path Params devem ser representados por entidades. `GET /api/house/houseId/devices/updates?from=from&to=to`. from e to não representam entidades do sistema.

# Section 15 - Advanced Architecture Topics
- Microservices
- Event Sourcing
- CQRS

# Section 16 - Soft Skills
- Ouvir
- Não pensar que você a pessoa mais inteligente do time
- Saber lidar com críticas utilizando lógica e fatos
- Seja inteligente ao invés de querer ter razão o tempo todo
- Conhecer as políticas da empresa
- Saber falar em público
- Sempre continue aprendendo (Keep Calm and Adapt or Die)
- Nunca esqueça que você trabalha com pessoas