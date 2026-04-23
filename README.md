# Guia de Transição: Kotlin → Golang

> **Versão:** 1.0.0
> **Última atualização:** Abril 2026
> **Público-alvo:** Time de desenvolvimento de software profissional
> **Propósito:** Guia estruturado de transição da linguagem Kotlin para Golang, com curadoria de ferramentas, padrões e estratégia de aprendizado por senioridade.

---

## Sumário

1. [Resumo Executivo](#1-resumo-executivo)
2. [Instruções para Leitura por IA](#2-instruções-para-leitura-por-ia)
3. [Pirâmide do Conhecimento Golang](#3-pirâmide-do-conhecimento-golang)
4. [Patterns e Boas Práticas em Golang](#4-patterns-e-boas-práticas-em-golang)
5. [Curadoria de Ferramentas de Teste](#5-curadoria-de-ferramentas-de-teste)
6. [IDEs para Desenvolvimento Profissional](#6-ides-para-desenvolvimento-profissional)

---

## 1. Resumo Executivo

Este documento foi criado como guia oficial de transição tecnológica para times que migram de **Kotlin** (JVM) para **Golang** em projetos profissionais. Ele abrange:

- Uma **pirâmide de conhecimento** dividida em três níveis — Júnior, Pleno e Sênior — com tópicos concretos que cada desenvolvedor deve dominar antes de avançar.
- Uma **curadoria de design patterns e system patterns** aplicáveis a projetos Golang do zero, com descrições que permitem gerar exemplos práticos via IA.
- Uma **curadoria de ferramentas de teste** cobrindo testes unitários, de integração, end-to-end e test containers, com orientações para geração de exemplos assistida por IA.
- Uma **avaliação de 4 IDEs** profissionais com foco em debug e análise de código, incluindo prompts para aprofundamento via IA.
- Um **protocolo de leitura por IA** que garante interpretação consistente por qualquer modelo de linguagem (Claude, Gemini, Copilot, entre outros).

O material não substitui a documentação oficial do Go. Ele funciona como mapa de navegação para que o time saiba **o que estudar, em que ordem, e com quais ferramentas**.

---

## 2. Instruções para Leitura por IA

A seção abaixo é um prompt estruturado. Ela deve ser fornecida a qualquer modelo de linguagem (Claude, Gemini, Copilot ou outro) **antes** de fazer perguntas sobre este documento. Isso garante que a IA interprete o conteúdo de forma padronizada, sem inferências livres.

### 2.1 Prompt para IA — Copie e use antes de enviar este documento

```text
CONTEXTO E REGRAS DE INTERPRETAÇÃO

Você está recebendo um documento técnico chamado "Guia de Transição: Kotlin → Golang".
Este documento é usado por um time de desenvolvimento profissional que está migrando
de Kotlin para Golang. Siga rigorosamente as instruções abaixo ao responder qualquer
pergunta sobre ele.

REGRAS OBRIGATÓRIAS:

1. FIDELIDADE AO DOCUMENTO
   - Responda exclusivamente com base no conteúdo presente neste documento.
   - Não invente, extrapole ou adicione informações que não estejam explicitamente escritas.
   - Se a resposta não estiver no documento, diga: "Esta informação não consta no documento."

2. ESTRUTURA DE SENIORIDADE
   - O documento define três níveis: Júnior (base), Pleno (meio), Sênior (topo).
   - Ao recomendar conteúdo ou responder dúvidas, sempre identifique a qual nível
     o tópico pertence.
   - Não misture conteúdo de níveis diferentes sem indicar claramente a transição.

3. GERAÇÃO DE EXEMPLOS
   - Ao longo do documento existem seções marcadas como "Prompt para Geração de Exemplo".
   - Quando o usuário pedir um exemplo sobre um desses tópicos, use o prompt fornecido
     no documento como base para gerar o código ou a explicação.
   - Os exemplos devem ser em Golang, idiomáticos, compiláveis e curtos (máximo 40 linhas,
     salvo se o usuário pedir mais).

4. COMPARAÇÕES KOTLIN vs GOLANG
   - Quando relevante, compare o conceito com o equivalente em Kotlin para facilitar
     a transição do desenvolvedor.
   - Use o formato: "Em Kotlin: [conceito]. Em Go: [conceito equivalente ou diferença]."

5. FORMATO DE RESPOSTA
   - Use tópicos curtos e diretos.
   - Inclua trechos de código somente quando solicitado ou quando o documento indicar
     um prompt de geração de exemplo.
   - Cite a seção do documento de onde a informação foi extraída.

6. FERRAMENTAS E IDEs
   - As recomendações de ferramentas e IDEs são curadas. Não substitua por alternativas
     não listadas no documento, a menos que o usuário peça explicitamente.

7. IDIOMA
   - Responda no mesmo idioma da pergunta do usuário.
   - O documento está em Português (Brasil), mas exemplos de código devem usar
     nomenclatura em Inglês, seguindo convenções da linguagem Go.
```

### 2.2 Como usar

1. Abra a IA de sua preferência (Claude, Gemini, Copilot, ChatGPT).
2. Cole o prompt da seção 2.1 como primeira mensagem.
3. Em seguida, envie este documento completo (cole o conteúdo ou faça upload do arquivo).
4. A partir daí, faça perguntas normalmente. A IA seguirá as regras definidas.

---

## 3. Pirâmide do Conhecimento Golang

A pirâmide está organizada de forma que cada nível **depende do anterior**. Um desenvolvedor Pleno deve dominar tudo do nível Júnior antes de avançar. O mesmo vale para a transição Pleno → Sênior.

---

### 3.1 Base — Nível Júnior

Fundamentos da linguagem e do ecossistema. O objetivo é que o desenvolvedor consiga escrever, compilar e testar código Go de forma autônoma.

**Sintaxe e Estruturas Fundamentais**

- Declaração de variáveis (`var`, `:=`, constantes com `const` e `iota`)
- Tipos primitivos: `int`, `float64`, `string`, `bool`, `byte`, `rune`
- Estruturas de controle: `if`, `for`, `switch`, `select`
- Funções: múltiplos retornos, funções variádicas, funções como valores
- Ponteiros: `*` e `&`, diferença para referências Kotlin

**Diferença mental vinda do Kotlin**

- Go não tem classes, herança, generics tradicionais (generics foram adicionados no Go 1.18 com type parameters), nem exceções (`try/catch`). O tratamento de erros é explícito via retorno `(resultado, error)`.
- Não há `null`. Existe o `nil`, que se aplica a ponteiros, slices, maps, channels, interfaces e funções.
- Não existe `data class`. Em Go, usa-se `struct`.

**Structs e Métodos**

- Definição de `struct` e composição (embedding) como alternativa a herança
- Métodos com receiver (value receiver vs pointer receiver)
- Quando usar ponteiro vs valor

**Coleções e Iteração**

- Arrays e Slices: criação, `append`, `copy`, slicing, capacidade vs tamanho
- Maps: criação, leitura com verificação de existência (comma ok idiom)
- Iteração com `range`

**Tratamento de Erros**

- O padrão `if err != nil`
- Criação de erros com `errors.New` e `fmt.Errorf` com `%w` para wrapping
- `errors.Is` e `errors.As` para inspeção de erros

**Pacotes e Módulos**

- Estrutura de diretórios e convenção de nomes de pacotes
- `go mod init`, `go mod tidy`, `go.sum`
- Visibilidade por capitalização (exportado vs não-exportado)
- Importação e organização de imports

**Ferramentas Essenciais da CLI**

- `go build`, `go run`, `go test`, `go fmt`, `go vet`
- `go doc` para consulta de documentação local

**Primeiro Contato com Testes**

- Arquivos `_test.go` e função `TestXxx(t *testing.T)`
- `t.Error`, `t.Fatal`, `t.Run` para subtestes
- Executar testes com `go test ./...`

---

### 3.2 Meio — Nível Pleno

Domínio de concorrência, interfaces, padrões de projeto e capacidade de estruturar projetos reais.

**Interfaces e Polimorfismo**

- Interfaces implícitas: qualquer tipo que implementa os métodos satisfaz a interface
- Interface vazia `any` (alias para `interface{}`) e type assertions
- Type switch para despacho por tipo
- Interfaces pequenas e compostas (princípio do Go: "Accept interfaces, return structs")
- Comparação com interfaces Kotlin: em Kotlin, a implementação é explícita (`class X : Interface`); em Go, é implícita (duck typing)

**Concorrência**

- Goroutines: lançamento com `go`, custo de memória (~2KB por goroutine)
- Channels: buffered vs unbuffered, direção (`chan<-`, `<-chan`)
- `select` para multiplexação de channels
- Padrões: fan-in, fan-out, pipeline, worker pool
- `sync.WaitGroup`, `sync.Mutex`, `sync.RWMutex`, `sync.Once`
- `context.Context`: cancelamento, timeout, deadline, propagação de valores
- Comparação com Kotlin: Coroutines do Kotlin são cooperativas e baseadas em suspensão; Goroutines são preemptivas e gerenciadas pelo runtime do Go

**Generics (Go 1.18+)**

- Type parameters em funções e tipos
- Constraints: `any`, `comparable`, constraints customizadas
- Quando usar e quando evitar (Go favorece simplicidade sobre abstração)

**Organização de Projeto**

- Estrutura de diretórios: `cmd/`, `internal/`, `pkg/` (quando faz sentido)
- Separação por domínio vs separação por camada técnica
- O anti-pattern de excesso de pacotes vs pacotes muito grandes

**Trabalho com JSON e HTTP**

- `encoding/json`: tags de struct, `Marshal`, `Unmarshal`, `json.Decoder`
- `net/http`: handlers, middleware, `http.ServeMux` (Go 1.22+ com routing melhorado)
- Frameworks opinados vs biblioteca padrão: quando cada abordagem vale

**Testes Intermediários**

- Table-driven tests (padrão idiomático do Go)
- Mocks manuais via interfaces
- `httptest` para testes de handlers HTTP
- `testify` para asserções e mocks

**Logging e Observabilidade Básica**

- `log/slog` (structured logging, Go 1.21+)
- Diferença conceitual do SLF4J/Logback do mundo Kotlin/JVM

---

### 3.3 Topo — Nível Sênior

Arquitetura, performance, resiliência e decisões de design em escala.

**Design de APIs e Contratos**

- Design de APIs RESTful idiomáticas em Go
- gRPC com Protocol Buffers: definição de serviços, geração de código, streaming
- Versionamento de API e backward compatibility
- Middleware chains e composição de handlers

**Performance e Profiling**

- `pprof`: CPU profiling, memory profiling, goroutine profiling
- `go tool trace` para análise de concorrência
- Benchmark tests com `testing.B`
- Escape analysis: entender quando o compilador aloca no heap vs stack
- Redução de alocações: reutilização com `sync.Pool`, pré-alocação de slices

**Padrões Avançados de Concorrência**

- Graceful shutdown com `context` e `os/signal`
- Rate limiting com `golang.org/x/time/rate`
- Circuit breaker e retry com backoff exponencial
- Semáforos com channels buffered
- Leak detection de goroutines em testes

**Resiliência e Operações**

- Health checks, readiness e liveness probes
- Métricas com Prometheus (`prometheus/client_golang`)
- Tracing distribuído com OpenTelemetry
- Feature flags e deploy progressivo (canary, blue-green)

**Segurança**

- Sanitização de inputs
- Gerenciamento de segredos (nunca em código)
- TLS, CORS, rate limiting, autenticação/autorização (JWT, OAuth2)
- Análise estática de segurança com `gosec`

**Decisões Arquiteturais**

- Monolito modular vs microserviços: critérios de decisão
- Event-driven architecture com mensageria (Kafka, NATS, RabbitMQ)
- CQRS e Event Sourcing: quando aplicar
- Database per service vs shared database

**Code Review e Qualidade**

- `golangci-lint` com configuração customizada
- Documentação com `godoc` e exemplos testáveis (`Example` functions)
- Dependency injection manual vs frameworks (Wire, fx)
- Estratégia de versionamento semântico para bibliotecas internas

---

## 4. Patterns e Boas Práticas em Golang

Esta seção apresenta uma curadoria de patterns relevantes para um projeto Go do zero. Cada pattern inclui uma descrição contextualizada e um prompt que pode ser usado para gerar exemplos via IA.

---

### 4.1 Design Patterns

#### 4.1.1 Functional Options

Padrão idiomático do Go para configuração flexível de structs sem precisar de builders ou construtores com dezenas de parâmetros. Usa funções como argumentos variádicos para aplicar configurações opcionais.

Em Kotlin, o equivalente seria um builder pattern ou named arguments com valores default. Em Go, Functional Options é a solução aceita pela comunidade.

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go do pattern Functional Options. Crie um struct "Server"
com campos Host, Port e Timeout. Implemente 3 option functions: WithHost,
WithPort e WithTimeout. Inclua uma função construtora NewServer que aceita
options variádicas. Mostre o uso com e sem opções. Máximo 35 linhas.
```

#### 4.1.2 Repository Pattern

Abstração da camada de persistência via interface. Permite trocar a implementação concreta (PostgreSQL, MongoDB, in-memory) sem alterar a lógica de negócio. Em Go, isso é feito com interfaces pequenas e injeção de dependência manual.

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go do Repository Pattern. Defina uma interface UserRepository
com métodos FindByID, Save e Delete. Implemente uma versão in-memory usando map.
Mostre uma função de serviço que recebe a interface como dependência. Máximo 40 linhas.
```

#### 4.1.3 Strategy Pattern

Define uma família de algoritmos encapsulados em funções ou interfaces, permitindo trocar o comportamento em tempo de execução. Em Go, é comum implementar com interfaces de um único método ou com `func` types.

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go do Strategy Pattern usando func types. Crie um tipo
PricingStrategy func(basePrice float64) float64. Implemente 3 estratégias:
RegularPricing, DiscountPricing e PremiumPricing. Mostre uma função
CalculateTotal que recebe a estratégia como parâmetro. Máximo 30 linhas.
```

#### 4.1.4 Decorator / Middleware Pattern

Encapsulamento de comportamento adicional (logging, autenticação, métricas) ao redor de uma função ou handler sem modificar seu código original. Em Go, o padrão de middleware HTTP é o exemplo canônico.

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go do Middleware Pattern para HTTP. Crie dois middlewares:
LoggingMiddleware (que loga método e path) e AuthMiddleware (que verifica um
header "X-API-Key"). Mostre como compor os middlewares ao redor de um handler
usando a assinatura func(http.Handler) http.Handler. Máximo 40 linhas.
```

#### 4.1.5 Factory Pattern

Criação de objetos sem expor a lógica de construção. Em Go, é implementado com funções construtoras (`NewXxx`) que retornam interfaces, ocultando a implementação concreta.

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go do Factory Pattern. Defina uma interface Notifier com
método Send(message string) error. Implemente EmailNotifier e SlackNotifier.
Crie uma função NewNotifier(channel string) Notifier que retorna a
implementação correta com base no argumento. Máximo 35 linhas.
```

---

### 4.2 System Patterns

#### 4.2.1 Circuit Breaker

Protege o sistema contra chamadas repetidas a serviços degradados. Quando o número de falhas excede um limiar, o circuito "abre" e rejeita chamadas por um período, evitando cascata de falhas. Depois de um tempo, tenta novamente (half-open).

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo simplificado em Go de um Circuit Breaker. Implemente um struct
CircuitBreaker com estados Closed, Open e HalfOpen. Inclua campos para
maxFailures, timeout e contadores. Implemente um método Execute que recebe uma
func() error e controla a transição de estados. Máximo 50 linhas.
Não use bibliotecas externas.
```

#### 4.2.2 Retry com Backoff Exponencial

Reexecuta operações que falharam com intervalos crescentes entre tentativas, evitando sobrecarregar o serviço remoto. Pode incluir jitter (variação aleatória) para evitar thundering herd.

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go de uma função Retry com backoff exponencial e jitter.
A função deve receber: a operação (func() error), número máximo de tentativas
e o intervalo base. Use time.Sleep entre tentativas. Máximo 25 linhas.
```

#### 4.2.3 Graceful Shutdown

Permite que a aplicação finalize requisições em andamento antes de encerrar o processo, respondendo a sinais do sistema operacional (SIGTERM, SIGINT). Essencial para ambientes containerizados (Kubernetes).

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go de Graceful Shutdown para um servidor HTTP. Use
os/signal para capturar SIGINT e SIGTERM. Use context.WithTimeout para dar
um prazo de 10 segundos para conexões finalizarem. Inclua o fluxo completo:
iniciar servidor em goroutine, aguardar sinal, chamar server.Shutdown(ctx).
Máximo 35 linhas.
```

#### 4.2.4 Health Check Pattern

Expõe endpoints que indicam se a aplicação está viva (liveness) e pronta para receber tráfego (readiness). Orquestradores como Kubernetes usam esses endpoints para decidir se devem reiniciar ou rotear tráfego para o pod.

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go de Health Check com dois endpoints: /healthz (liveness,
sempre retorna 200) e /readyz (readiness, verifica conexão com banco de dados
simulado). Use net/http padrão. Inclua um struct HealthChecker com método
CheckDB() error. Máximo 35 linhas.
```

---

### 4.3 Boas Práticas Gerais

**Tratamento de Erros** — Sempre trate erros explicitamente. Nunca use `_` para ignorar um `error` retornado, exceto em casos documentados e justificados. Use `fmt.Errorf("contexto: %w", err)` para adicionar contexto preservando a cadeia de erros.

**Naming** — Nomes curtos e descritivos. Variáveis de escopo curto podem ser abreviadas (`r` para reader, `w` para writer). Nomes de pacotes são singulares e lowercase, sem underscores. Acrônimos ficam em caixa consistente (`HTTPClient`, não `HttpClient`).

**Composição sobre herança** — Go não tem herança. Use embedding de structs para reutilização e interfaces pequenas para polimorfismo. Prefira interfaces de 1-2 métodos.

**Concorrência segura** — Não compartilhe memória; comunique-se via channels. Se usar memória compartilhada, proteja com `sync.Mutex`. Passe `context.Context` como primeiro parâmetro de funções que podem ser canceladas.

**Dependências** — Mantenha o `go.mod` limpo. Avalie bibliotecas externas criticamente: a stdlib do Go é muito completa. Prefira soluções da biblioteca padrão quando a diferença de funcionalidade for marginal.

---

## 5. Curadoria de Ferramentas de Teste

### 5.1 Testes Unitários

#### 5.1.1 testing (biblioteca padrão)

O pacote `testing` é a base de todo teste em Go. Não requer instalação e é integrado ao toolchain. Arquivos de teste terminam com `_test.go` e funções de teste começam com `Test`. Suporta subtestes (`t.Run`), testes paralelos (`t.Parallel`) e benchmarks (`testing.B`).

A prática idiomática em Go é usar **table-driven tests**: uma slice de structs com inputs e outputs esperados, iterada com `t.Run`.

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go de table-driven test usando apenas o pacote testing.
Teste uma função Divide(a, b float64) (float64, error) que retorna erro para
divisão por zero. Inclua pelo menos 4 casos de teste na tabela, incluindo o
caso de erro. Use t.Run para subtestes nomeados. Máximo 35 linhas.
```

#### 5.1.2 testify

Biblioteca de terceiros (`github.com/stretchr/testify`) que fornece asserções legíveis (`assert.Equal`, `assert.NoError`, `assert.Contains`) e suíte de mocks. Reduz boilerplate em comparação ao `testing` puro.

Possui dois pacotes principais: `assert` (não interrompe o teste em caso de falha) e `require` (interrompe imediatamente).

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go usando testify/assert e testify/require. Teste a mesma
função Divide do exemplo anterior, mas agora usando assert.Equal para validar
resultados e require.NoError para validar ausência de erro. Mostre a diferença
entre assert (continua) e require (para). Máximo 30 linhas.
```

#### 5.1.3 gomock / mockgen

Ferramenta oficial do Go (`go.uber.org/mock`) para geração automática de mocks a partir de interfaces. Usa `mockgen` para gerar código e `gomock.Controller` para validar expectativas de chamadas durante o teste.

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go usando gomock. Defina uma interface EmailSender com
método Send(to, body string) error. Mostre: (1) o comando mockgen para gerar
o mock, (2) um teste que configura expectativa com EXPECT().Send() e verifica
que o serviço chama o sender corretamente. Máximo 35 linhas de código Go
(exclua o output do mockgen).
```

---

### 5.2 Testes de Integração

#### 5.2.1 Build Tags para Separação

Go permite separar testes de integração dos unitários usando build tags. A convenção é adicionar `//go:build integration` no topo do arquivo e executar com `go test -tags=integration ./...`. Isso evita que testes lentos rodem no ciclo rápido de desenvolvimento.

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go de teste de integração usando build tags. Crie um arquivo
user_repository_integration_test.go com a tag //go:build integration. O teste
deve conectar a um banco de dados (pode simular com um map global), inserir um
registro e consultar. Mostre o comando para executar apenas os testes de
integração. Máximo 30 linhas.
```

#### 5.2.2 httptest

Pacote da biblioteca padrão que cria servidores HTTP reais em loopback para testes. Permite testar clientes HTTP contra servidores mock sem abrir portas reais na rede.

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go usando httptest.NewServer para testar um cliente HTTP.
Crie um servidor mock que retorna um JSON de usuário. Implemente um
UserClient com método GetUser(id string) que faz GET ao servidor. Teste
que o client parseia a resposta corretamente. Máximo 35 linhas.
```

---

### 5.3 Testes End-to-End (E2E)

#### 5.3.1 Estratégia para E2E em Go

Testes E2E em Go geralmente são executados como testes de integração com build tags separadas (`//go:build e2e`). A aplicação é iniciada como subprocesso ou via `httptest.Server` com o router completo configurado. As asserções são feitas via chamadas HTTP reais ao servidor.

Para testes E2E que envolvem frontend ou fluxos de browser, ferramentas como **chromedp** (controle de Chrome headless via Go) ou **Playwright** (via CLI externa) podem ser integradas.

**Prompt para Geração de Exemplo:**

```text
Gere um exemplo em Go de teste E2E. Inicie a aplicação completa usando
httptest.NewServer com o router principal (pode ser um http.ServeMux com
3 endpoints: POST /users, GET /users/{id}, GET /health). O teste deve:
criar um usuário, recuperá-lo e validar os dados. Use a tag //go:build e2e.
Máximo 50 linhas.
```

---

### 5.4 Testcontainers

#### 5.4.1 testcontainers-go

Biblioteca (`github.com/testcontainers/testcontainers-go`) que gerencia containers Docker durante testes. Permite subir bancos de dados, filas, caches e qualquer serviço necessário com ciclo de vida atrelado ao teste. O container é criado antes do teste e destruído depois.

Suporta módulos pré-configurados para PostgreSQL, MySQL, Redis, Kafka, MongoDB, entre outros.

**Prompt para Geração de Exemplo — Básico:**

```text
Gere um exemplo em Go usando testcontainers-go para subir um container
PostgreSQL durante um teste. Use o módulo postgres do testcontainers-go.
O teste deve: iniciar o container, obter a connection string, conectar via
database/sql com o driver pgx, criar uma tabela, inserir um registro e
consultar. Limpe o container com defer. Máximo 45 linhas.
```

**Prompt para Geração de Exemplo — Avançado:**

```text
Gere um exemplo em Go usando testcontainers-go com Redis. Suba um container
Redis, conecte usando go-redis, faça Set e Get de um valor. Mostre como
configurar um timeout para o container e como usar testcontainers.CleanupContainer.
Máximo 35 linhas.
```

---

### 5.5 Resumo Comparativo de Ferramentas de Teste

| Tipo de Teste | Ferramenta | Instalação | Quando Usar |
|---|---|---|---|
| Unitário | `testing` (stdlib) | Nenhuma | Sempre — base de toda suíte de testes |
| Unitário | `testify` | `go get github.com/stretchr/testify` | Quando quiser asserções mais legíveis |
| Unitário (mocks) | `gomock` | `go install go.uber.org/mock/mockgen` | Quando precisar de mocks gerados automaticamente |
| Integração | `httptest` (stdlib) | Nenhuma | Testes de handlers e clientes HTTP |
| Integração | Build tags | Nenhuma | Separar testes lentos dos rápidos |
| E2E | `httptest` + router completo | Nenhuma | Testes de fluxo completo da API |
| Containers | `testcontainers-go` | `go get github.com/testcontainers/testcontainers-go` | Testes com dependências reais (DB, cache, fila) |

---

## 6. IDEs para Desenvolvimento Profissional

As quatro IDEs abaixo foram selecionadas por oferecerem suporte robusto a Go, incluindo ferramentas de debug integradas, análise estática de código e produtividade para times profissionais.

---

### 6.1 GoLand (JetBrains)

**Descrição:** IDE dedicada para Go da JetBrains. Oferece a experiência mais completa e pronta para uso, sem necessidade de configurar extensões. Inclui debugger gráfico baseado em Delve, profiler integrado, refatoração avançada, navegação por código, suporte a testes com UI gráfica e integração com banco de dados.

**Destaques para Debug:** Breakpoints condicionais, watchpoints em variáveis, inspeção de goroutines em tempo real, avaliação de expressões durante pausa, debug remoto via SSH ou Docker, e debug de testes diretamente pelo ícone de execução no editor.

**Licença:** Comercial (30 dias trial, licença paga individual ou empresa).

**Contexto para quem vem do Kotlin:** Se o time já usa IntelliJ IDEA para Kotlin, GoLand terá a mesma interface e atalhos, eliminando curva de adaptação na IDE.

**Prompt para Geração de Exemplo — Debug Simples:**

```text
Descreva passo a passo como fazer debug de uma função Go no GoLand. A função
recebe um slice de inteiros e retorna a soma. Mostre: como colocar um breakpoint,
iniciar o debugger, inspecionar o valor de variáveis locais a cada iteração do
loop e usar o painel "Variables". Inclua screenshots textuais (descrição do que
aparece na tela) se possível. Foco no fluxo para um desenvolvedor acostumado
com IntelliJ/Kotlin.
```

**Prompt para Geração de Exemplo — Debug Complexo:**

```text
Descreva como debugar um cenário de race condition no GoLand. O cenário: 3
goroutines incrementam um contador compartilhado sem mutex. Mostre: como usar
o detector de race condition (-race flag), como colocar breakpoints em goroutines
específicas, como usar o painel "Goroutines" para inspecionar o estado de cada
goroutine, e como identificar o ponto de conflito. Inclua o comando de terminal
equivalente para comparação.
```

---

### 6.2 Visual Studio Code (VS Code) + Go Extension

**Descrição:** Editor open-source da Microsoft com suporte a Go via extensão oficial (`golang.go`). A extensão instala automaticamente ferramentas como `gopls` (language server), `dlv` (debugger Delve), `staticcheck` (linter) e `goimports`. Excelente custo-benefício, com grande ecossistema de extensões adicionais.

**Destaques para Debug:** Configuração via `launch.json`, breakpoints, step-in/out/over, inspeção de variáveis, debug de testes individuais com CodeLens, debug de aplicações em containers via Remote Development, e integração com terminal integrado.

**Licença:** Gratuito e open-source.

**Prompt para Geração de Exemplo — Debug Simples:**

```text
Descreva passo a passo como configurar e usar o debugger de Go no VS Code.
Inclua: instalação da extensão Go, configuração do launch.json para um
programa main.go, como colocar breakpoints, iniciar debug com F5 e inspecionar
variáveis no painel lateral. Foco em um desenvolvedor que nunca usou o
debugger do VS Code antes.
```

**Prompt para Geração de Exemplo — Debug Complexo:**

```text
Descreva como debugar um servidor HTTP Go no VS Code. O servidor tem 3 endpoints.
Mostre: configuração do launch.json para attach em processo rodando, como fazer
debug de um request específico colocando breakpoint no handler, como inspecionar
o http.Request e o ResponseWriter durante a pausa, e como usar conditional
breakpoints para parar apenas quando um query parameter específico está presente.
```

---

### 6.3 Neovim + gopls + nvim-dap

**Descrição:** Editor terminal altamente customizável, usado por desenvolvedores que preferem fluxo keyboard-driven. Com `gopls` (LSP), `nvim-dap` (Debug Adapter Protocol) e `nvim-dap-ui`, oferece uma experiência completa de desenvolvimento Go sem sair do terminal. Requer configuração inicial, mas resulta em um ambiente extremamente rápido.

**Destaques para Debug:** Integração com Delve via DAP, breakpoints, step-through, inspeção de variáveis em floating windows, debug de testes via keybinding customizado, e REPL interativo do Delve acessível de dentro do Neovim.

**Licença:** Gratuito e open-source.

**Prompt para Geração de Exemplo — Debug Simples:**

```text
Descreva como configurar debug de Go no Neovim. Inclua: instalação de
nvim-dap e nvim-dap-ui via lazy.nvim, configuração do adapter para Delve
(dlv), mapeamento de teclas para toggle breakpoint, continue, step over e
step into. Mostre o fluxo de debug de uma função simples que processa um
slice. Inclua o trecho de configuração Lua necessário. Máximo 50 linhas de config.
```

**Prompt para Geração de Exemplo — Debug Complexo:**

```text
Descreva como debugar um teste de integração com testcontainers-go no Neovim
usando nvim-dap. Mostre: como configurar o adapter para rodar testes
específicos (equivalente ao dlv test), como definir breakpoints dentro do
teste, como inspecionar o estado do container e da conexão ao banco durante
a pausa. Inclua a configuração DAP necessária para este cenário.
```

---

### 6.4 Zed

**Descrição:** Editor de código de nova geração focado em performance e colaboração, escrito em Rust. Tem suporte nativo a Go via `gopls`, com completions, diagnósticos e navegação rápida. Suporta debug via integração com DAP (Debug Adapter Protocol). Indicado para desenvolvedores que buscam uma alternativa moderna ao VS Code com menor consumo de recursos.

**Destaques para Debug:** Suporte a Delve via DAP, breakpoints, painel de variáveis, console de debug. A experiência de debug está em evolução ativa e melhora a cada release.

**Licença:** Gratuito e open-source.

**Prompt para Geração de Exemplo — Debug Simples:**

```text
Descreva como configurar e usar o debug de Go no Zed. Inclua: verificação
de que o language server gopls está ativo, configuração do debug adapter
para Delve, como colocar breakpoints e iniciar uma sessão de debug de um
programa main.go. Descreva os painéis disponíveis durante a sessão de debug.
```

**Prompt para Geração de Exemplo — Debug Complexo:**

```text
Descreva como fazer debug de um worker pool em Go usando Zed. O cenário:
5 goroutines consomem jobs de um channel e processam. Mostre como acompanhar
a execução de goroutines individuais, como usar conditional breakpoints para
pausar em um job específico e como inspecionar o estado do channel durante
a pausa.
```

---

### 6.5 Resumo Comparativo de IDEs

| IDE | Licença | Debug Integrado | Melhor Para |
|---|---|---|---|
| GoLand | Comercial | Completo (Delve, UI gráfica, profiler) | Times que já usam JetBrains e querem zero configuração |
| VS Code | Gratuito | Completo (Delve via extensão, launch.json) | Versatilidade, grande ecossistema de extensões |
| Neovim | Gratuito | Completo (Delve via nvim-dap, configuração manual) | Desenvolvedores keyboard-driven que querem máximo controle |
| Zed | Gratuito | Em evolução (Delve via DAP) | Quem busca performance e uma alternativa moderna |

---

## Apêndice: Referências Oficiais

- Documentação oficial do Go: https://go.dev/doc/
- Effective Go: https://go.dev/doc/effective_go
- Go Blog: https://go.dev/blog/
- Go by Example: https://gobyexample.com/
- Go Playground: https://go.dev/play/
- Go Proverbs (Rob Pike): https://go-proverbs.github.io/
- Testcontainers for Go: https://golang.testcontainers.org/
- Delve Debugger: https://github.com/go-delve/delve
- GoLand Docs: https://www.jetbrains.com/help/go/
- VS Code Go Extension: https://marketplace.visualstudio.com/items?itemName=golang.Go

---

> **Nota final:** Este documento é um guia vivo. Atualize-o conforme o time evolui na adoção do Go. Os prompts de geração de exemplo foram escritos para serem usados com qualquer IA generativa e devem produzir resultados consistentes independentemente do modelo utilizado.
