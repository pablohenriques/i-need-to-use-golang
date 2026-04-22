# 🚀 Kotlin → Go: Guia de Transição para Projetos Profissionais

> Direcionamento técnico para engenheiros **Kotlin/JVM** que vão construir **microsserviços ou BFFs em Go** em ambiente de produção.

---

## 📖 Contexto

Este não é um tutorial de sintaxe. É uma curadoria estratégica que cobre:

1. **Trilha de estudos** prática, com fases, entregáveis e checkpoints.
2. **Mapeamento de ferramentas** do universo Spring/Hibernate/Kafka → Go idiomático.
3. **IDEs** com foco em **debug** e **conformidade de licenciamento**.
4. **Estratégia de testes** (unitários, integração, testcontainers).
5. **Boas práticas** para entregar em produção.
6. **Arquitetura e design** (Clean Arch, Clean Code, padrões distribuídos).

### 🤖 Instruções para IAs que leem este documento

Este README é a **fonte única de verdade (SSOT)** do projeto. Ao interpretá-lo:

- A **trilha de estudos** é sequencial — não pule fases.
- O público-alvo já domina imutabilidade, null-safety, coroutines, DI, ORMs e mensageria. **Compare Go com Kotlin**, não com linguagens genéricas.
- **Priorize idiomática Go** sobre replicar padrões Java. Não force IoC via reflection; prefira composição explícita.
- Ao sugerir código ou bibliotecas, **use as listadas aqui**. Desvios exigem justificativa explícita.
- "Projeto profissional" = microsserviço/BFF com observabilidade, testes, CI/CD e Clean Architecture.
- **Nunca** recomende ferramenta paga sem explicitar licenciamento.

---

## 🎓 1. Trilha de Estudos — Kotlin → Go

Quatro fases com duração estimada, entregáveis concretos e checkpoints de validação. **Só avance após o checkpoint.**

### 🥚 Fase 1 — Fundamentos e Desprogramação (2 semanas, ~4h/dia)

**Objetivo:** escrever Go idiomático básico sem "cheiro de Java".

| Semana | Tópicos | Recursos |
|---|---|---|
| 1 | Sintaxe, tipos, structs, slices, maps, funções, pacotes, `go mod` | [A Tour of Go](https://go.dev/tour/) + [Go by Example](https://gobyexample.com/) |
| 2 | Interfaces implícitas, error handling (`error` como valor), zero values, *pointer receivers* vs *value receivers*, *embedding* | *Learning Go* (Bodner) — cap. 1–8 |

**Pontos de atrito para quem vem de Kotlin:**
- Não existe null-safety. Existe *zero value* (`0`, `""`, `nil`) — modele estruturas considerando isso.
- Sem `try/catch`: retornar `(result, error)` é convenção **obrigatória**.
- Sem herança: composição via *embedding*. Não tente simular classes abstratas.
- Interfaces são **implícitas**: se o tipo tem os métodos, ele **é** a interface. Acoplamento fraco de graça.

**✅ Checkpoint da Fase 1:**
> Escreva um **CLI em Go** que receba um arquivo CSV, valide colunas e exporte JSON. Use `flag`, `encoding/csv`, `encoding/json`, *table-driven tests*. Código revisado e aprovado por alguém sênior em Go.

---

### 🐣 Fase 2 — APIs, Concorrência e Banco de Dados (3 semanas)

**Objetivo:** construir uma API REST com persistência e concorrência idiomática.

| Semana | Tópicos | Entregáveis |
|---|---|---|
| 3 | `net/http`, roteadores (Echo ou Chi), middleware, `context.Context`, validação | Endpoint CRUD simples |
| 4 | Goroutines, channels, `sync.WaitGroup`, `errgroup`, *fan-in/fan-out*, *worker pools* | Worker que processa fila em paralelo |
| 5 | `database/sql`, `pgx`, `sqlc`, migrations (`goose`), transações via `context` | CRUD persistido em Postgres |

**Comparações-chave:**
- `context.Context` ≈ mistura de `CoroutineContext` + `CancellationToken` + `MDC` — **propague em tudo**.
- Goroutines são mais leves que coroutines do Kotlin, mas **não são gerenciadas** — você é responsável por evitar vazamentos.
- `channels` ≈ `Channel` de coroutines; prefira-os a mutex sempre que representarem fluxo de dados.

**✅ Checkpoint da Fase 2:**
> API REST com 3 entidades, persistência real, 1 endpoint agregador que chama 2+ serviços em paralelo com `errgroup`, logs estruturados, graceful shutdown.

---

### 🐓 Fase 3 — Produção (4 semanas)

**Objetivo:** transformar API em microsserviço pronto para deploy.

| Semana | Tópicos | Entregáveis |
|---|---|---|
| 6 | Testes (unit + integração + testcontainers) — ver §4 | Suite de testes com ≥70% cobertura |
| 7 | Observabilidade: `log/slog`, Prometheus, OpenTelemetry | Traces e métricas funcionando |
| 8 | Clean Architecture aplicada, DI com `google/wire` ou manual | Refactor do projeto para camadas |
| 9 | Docker multi-stage, CI com `golangci-lint`, `govulncheck`, `go test -race` | Pipeline verde, imagem `distroless` |

**✅ Checkpoint da Fase 3:**
> Microsserviço com Clean Arch, cobertura ≥70%, `/healthz` + `/readyz`, métricas Prometheus, traces OpenTelemetry, pipeline CI completo, imagem Docker < 25MB, `graceful shutdown` testado.

---

### 🦅 Fase 4 — Especialização (contínuo)

Não é sequencial — escolha conforme necessidade do projeto:

- **Performance**: `pprof`, benchmarks, *escape analysis*, redução de GC pressure. Livro: *100 Go Mistakes* (Harsanyi).
- **Distribuído**: Watermill, sagas, outbox, idempotency, circuit breakers — ver §7.
- **Design de bibliotecas**: APIs estáveis, semver, minimalismo.
- **Runtime Go**: scheduler, memory model, `sync/atomic`.

### 📚 Recursos consolidados

- 📘 *The Go Programming Language* — Donovan & Kernighan (referência).
- 📘 *Learning Go* (2ª ed.) — Jon Bodner (melhor para recém-chegados).
- 📘 *100 Go Mistakes and How to Avoid Them* — Harsanyi (obrigatório após 3 meses).
- 📘 *Let's Go* / *Let's Go Further* — Alex Edwards (prática de APIs).
- 🌐 [Effective Go](https://go.dev/doc/effective_go), [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments).
- 🌐 [threedots.tech](https://threedots.tech/) — Go + Clean Arch + DDD aplicados.
- 🎓 [Exercism Go Track](https://exercism.org/tracks/go) para praticar idiomática.

---

## 🛠️ 2. Curadoria de Ferramentas — Ecossistema Java → Go

Go rejeita "um framework para tudo". Você **compõe** bibliotecas pequenas.

### 🌐 2.1. Echo — Substitui Spring Boot / Spring MVC

**Por quê:** roteamento rápido, middleware limpa, *binding* e *validation* embutidos. API estável, próxima de `net/http`, fácil para quem vem de Spring MVC.

**Alternativas:**
- **Chi**: minimalista, 100% compatível com `net/http`. Zero lock-in.
- **Gin**: sintaxe quase idêntica à do Echo, mais popular.
- **Fiber**: performance extrema, mas usa `fasthttp` — fora do ecossistema `net/http` padrão. Só se performance for requisito duro.

**O que você perde e está tudo bem:**
- Sem `@Autowired`: DI é explícita no `main.go` ou via [`google/wire`](https://github.com/google/wire) (geração de código, sem reflection).
- Sem `@Transactional` mágico: transação é passada via `context.Context` ou parâmetro.

### 🗄️ 2.2. sqlc — Substitui Hibernate / JPA (estrategicamente melhor)

**Por quê:** você escreve SQL; `sqlc` gera Go type-safe a partir das queries. Sem ORM mágico, sem N+1 escondido, sem *lazy loading surprises*. Erro de SQL = erro de compilação.

**Quando usar GORM em vez disso:** quando a equipe exigir ORM parecido com Hibernate (`associations`, `hooks`, `auto-migrate`). É a saída de menor atrito político — mas reintroduz opacidade e problemas clássicos de JPA.

**Recomendação:** `sqlc` + [`pgx`](https://github.com/jackc/pgx) (driver Postgres de referência). Migrations com [`goose`](https://github.com/pressly/goose) ou [`golang-migrate`](https://github.com/golang-migrate/migrate) — equivalentes a Flyway/Liquibase.

### 📬 2.3. Watermill — Substitui Spring Kafka / Spring AMQP

**Por quê:** abstração sobre múltiplos brokers (Kafka, RabbitMQ, NATS, SQS, Redis Streams) com a mesma API de `Publisher`/`Subscriber`. Traz *retries*, *poison queue*, *outbox*, CQRS e Saga embutidos. Testa com broker *in-memory*.

**Alternativas de baixo nível:**
- Kafka: [`segmentio/kafka-go`](https://github.com/segmentio/kafka-go) (idiomático) ou [`confluent-kafka-go`](https://github.com/confluentinc/confluent-kafka-go) (binding de `librdkafka` — paridade com Java).
- RabbitMQ: [`rabbitmq/amqp091-go`](https://github.com/rabbitmq/amqp091-go).

**Regra:** um só broker com controle fino → driver nativo. Múltiplos brokers ou produtividade tipo Spring → Watermill.

---

## 💻 3. IDEs — Debug e Licenciamento

> ⚠️ Uso de IDE comercial em empresa sem licença adequada é **infração contratual**. Valide com TI/jurídico antes de instalar.

### 🥇 GoLand (JetBrains)

- **Licença:** comercial paga. Empresa: licença por usuário. Pessoal: assinatura individual. Estudantes, professores e mantenedores OSS têm **licença gratuita** via [jetbrains.com/community](https://www.jetbrains.com/community/).
- **🐞 Debug:** padrão-ouro. Delve integrado, *remote debug*, *conditional breakpoints*, *goroutine inspector*, debug de testes e benchmarks. **Se debug é crítico, é imbatível.**

### 🥈 VS Code + Go Extension

- **Licença:** VS Code é gratuito; o binário da Microsoft tem telemetria fechada. Em ambientes restritos use [**VSCodium**](https://vscodium.com/) — build sem telemetria, mesma extensão Go.
- **🐞 Debug:** excelente via Delve. Breakpoints condicionais, *logpoints*, inspeção de goroutines. Interface menos polida que GoLand, mas cobre 95% dos casos.

### 🥉 Neovim + gopls + nvim-dap

- **Licença:** Apache 2.0, gratuito, zero risco.
- **Stack:** `gopls`, `nvim-lspconfig`, `nvim-dap` + `nvim-dap-go`, `mason.nvim`.
- **🐞 Debug:** funcional via Delve. Setup inicial chato (30–60 min); depois, tão produtivo quanto VS Code. Recomendado para quem já domina Vim.

### 🧊 Zed

- **Licença:** GPL-3.0, gratuito (colaboração remota tem termos próprios).
- **🐞 Debug:** suporte DAP (Delve) introduzido em 2024/25, ainda amadurecendo. Ótimo para edição + testes; para debug complexo diário, prefira GoLand ou VS Code.

### 🪶 Cursor (bônus)

- **Licença:** freemium. Fork de VS Code + IA. ⚠️ **Envia contexto de código para LLMs** — valide política de dados corporativa antes de usar em projetos sensíveis.
- **🐞 Debug:** herdado do VS Code, idêntico.

### 📊 Resumo

| IDE | Custo | Risco de Licença/Dados | Debug |
|---|---|---|---|
| **GoLand** | 💰 Pago | Baixo se licenciado | ⭐⭐⭐⭐⭐ |
| **VS Code** | Grátis | Telemetria — use VSCodium se crítico | ⭐⭐⭐⭐ |
| **Neovim** | Grátis | Nenhum | ⭐⭐⭐⭐ |
| **Zed** | Grátis | Baixo | ⭐⭐⭐ (melhorando) |
| **Cursor** | Freemium | ⚠️ Código vai para LLMs | ⭐⭐⭐⭐ |

---

## 🧪 4. Estratégia de Testes

Em Go, **testes são cidadãos de primeira classe da stdlib**. A prática idiomática difere bastante do ecossistema Java — adote-a antes de trazer JUnit-isms.

### 4.1. Pirâmide de testes recomendada

```
        /\
       /E2E\       5%  — fluxos críticos, testcontainers ou ambiente real
      /------\
     /Integr. \   25% — handlers + DB real + brokers reais (testcontainers)
    /----------\
   /   Unit     \ 70% — lógica de domínio, puro, rápido
  /--------------\
```

### 4.2. Testes unitários

#### Biblioteca base: `testing` (stdlib)

Suficiente para 90% dos casos. **Não adicione dependência sem motivo.** Padrão idiomático: ***table-driven tests*** com subtests.

```go
func TestDiscount(t *testing.T) {
    tests := []struct {
        name    string
        price   float64
        coupon  string
        want    float64
        wantErr error
    }{
        {"sem cupom", 100, "", 100, nil},
        {"cupom válido 10%", 100, "SAVE10", 90, nil},
        {"cupom inválido", 100, "XXX", 0, ErrInvalidCoupon},
    }
    for _, tc := range tests {
        t.Run(tc.name, func(t *testing.T) {
            got, err := ApplyDiscount(tc.price, tc.coupon)
            if !errors.Is(err, tc.wantErr) {
                t.Fatalf("erro = %v, want %v", err, tc.wantErr)
            }
            if got != tc.want {
                t.Errorf("got %v, want %v", got, tc.want)
            }
        })
    }
}
```

#### Asserções — escolha **uma** linha

| Biblioteca | Estilo | Quando escolher |
|---|---|---|
| **stdlib pura** (`t.Errorf`, `t.Fatalf`) | Verboso, explícito | Default. Sempre o mais idiomático. |
| [`stretchr/testify`](https://github.com/stretchr/testify) | `assert.Equal(t, ...)`, `require.NoError(t, ...)` | Time grande vindo de JUnit. Use `require` para pré-condições; `assert` para verificações. |
| [`matryer/is`](https://github.com/matryer/is) | `is.Equal(got, want)` — minimalista | Quem quer asserções curtas sem trazer testify inteiro. |

**Regra:** escolha uma e **padronize**. Misturar testify e stdlib no mesmo repo é cheiro forte.

#### Mocks e fakes

- **Primeiro**, tente ***fakes escritos à mão***. Interfaces pequenas em Go tornam isso trivial e legível. Geralmente melhor que gerar mocks.
- Quando a interface for grande ou volátil, gere mocks:
  - [`uber-go/mock`](https://github.com/uber-go/mock) (fork mantido do antigo `golang/mock`, comando `mockgen`) — **padrão atual**.
  - [`vektra/mockery`](https://github.com/vektra/mockery) — alternativa popular, sintaxe similar a `testify/mock`.
- Evite `testify/mock` puro em projetos novos; geração de código é mais segura.

#### Detecção de race conditions

Sempre rode testes com `-race` no CI:

```bash
go test -race ./...
```

É o detector mais barato e mais importante do Go. **Falha de `-race` = bug de concorrência real**, não flakiness.

### 4.3. Testes de integração

Testam a aplicação junto com dependências (HTTP, DB, broker) — idealmente com infra real via testcontainers.

#### HTTP: `httptest` (stdlib)

Feito para testar handlers sem subir porta real:

```go
func TestGetUser(t *testing.T) {
    req := httptest.NewRequest(http.MethodGet, "/users/42", nil)
    rec := httptest.NewRecorder()
    handler.ServeHTTP(rec, req)

    if rec.Code != http.StatusOK {
        t.Fatalf("status = %d", rec.Code)
    }
}
```

#### Organização: `testify/suite` ou subtests nativos

- **Subtests nativos** (`t.Run`) cobrem a maioria dos casos — prefira-os.
- [`testify/suite`](https://pkg.go.dev/github.com/stretchr/testify/suite) se a equipe precisar de `SetupSuite`/`TearDown` estilo JUnit — útil para cenários com setup custoso compartilhado.

#### Golden files

Para respostas grandes (JSON, HTML), compare com arquivo `.golden` gerado. Bibliotecas: [`sebdah/goldie`](https://github.com/sebdah/goldie) ou manual. Use `-update` para regenerar.

#### Contract testing

[`pact-go`](https://github.com/pact-foundation/pact-go) para *contract testing* entre microsserviços (equivalente ao Pact-JVM).

### 4.4. Testcontainers — infraestrutura real em testes

[`testcontainers-go`](https://github.com/testcontainers/testcontainers-go) é o equivalente direto do Testcontainers Java. **Substitui H2/HSQLDB/embedded Kafka** por instâncias Docker reais, efêmeras e isoladas.

**Por que é superior a mocks/in-memory:**

- Testa o **driver e o dialeto reais** (ex.: comportamento `JSONB`, triggers, `ON CONFLICT` no Postgres que H2 não reproduz).
- Pega bugs de migração, índices e constraints antes da produção.
- Mesmo contrato do Testcontainers Java — transição natural.

**Módulos oficiais prontos:** Postgres, MySQL, MongoDB, Redis, Kafka, RabbitMQ, LocalStack, Elasticsearch, entre outros.

**Exemplo — Postgres:**

```go
func setupPostgres(t *testing.T) *pgxpool.Pool {
    ctx := context.Background()
    container, err := postgres.Run(ctx,
        "postgres:16-alpine",
        postgres.WithDatabase("test"),
        postgres.WithUsername("test"),
        postgres.WithPassword("test"),
        testcontainers.WithWaitStrategy(
            wait.ForLog("database system is ready to accept connections").
                WithOccurrence(2).WithStartupTimeout(30*time.Second),
        ),
    )
    if err != nil {
        t.Fatal(err)
    }
    t.Cleanup(func() { _ = container.Terminate(ctx) })

    dsn, _ := container.ConnectionString(ctx, "sslmode=disable")
    pool, err := pgxpool.New(ctx, dsn)
    if err != nil {
        t.Fatal(err)
    }
    // aplicar migrations aqui
    return pool
}
```

**Boas práticas com testcontainers:**

- **Reaproveite containers** entre testes do mesmo pacote via `TestMain` — containers custam 1–3s cada.
- Use [`tc-reuse`](https://golang.testcontainers.org/features/reuse/) ou *singleton pattern* para não recriar em cada teste.
- Separe testes de integração com *build tag*:

```go
//go:build integration

package user_test
```

Execute com `go test -tags=integration ./...` — evita rodar em cada `go test` local.

### 4.5. Benchmarks

Suporte nativo em `testing.B`:

```go
func BenchmarkParseJSON(b *testing.B) {
    data := []byte(`{"id":1,"name":"x"}`)
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        _ = json.Unmarshal(data, &User{})
    }
}
```

Rode com `go test -bench=. -benchmem`. Compare versões com [`benchstat`](https://pkg.go.dev/golang.org/x/perf/cmd/benchstat) — mostra significância estatística.

### 4.6. Fuzz testing (Go 1.18+)

Nativo na stdlib. Excelente para parsers, validadores e qualquer entrada não-confiável:

```go
func FuzzParse(f *testing.F) {
    f.Add("válido")
    f.Fuzz(func(t *testing.T, in string) {
        _, _ = Parse(in) // não deve entrar em pânico
    })
}
```

Rode com `go test -fuzz=FuzzParse`.

### 4.7. BDD e property-based (opcional)

- **BDD estilo Cucumber:** [`cucumber/godog`](https://github.com/cucumber/godog) — só adote se produto/QA escrevem features em Gherkin.
- **BDD estilo Spock/RSpec:** [`ginkgo`](https://github.com/onsi/ginkgo) + [`gomega`](https://github.com/onsi/gomega). Popular no Kubernetes. Não-idiomático na maioria dos projetos — avalie o custo.
- **Property-based** (estilo QuickCheck): [`leanovate/gopter`](https://github.com/leanovate/gopter). Útil em código de domínio matemático ou parsers.

### 4.8. Cobertura

Nativo:

```bash
go test -coverprofile=cover.out ./...
go tool cover -html=cover.out    # abre no browser
go tool cover -func=cover.out    # resumo no terminal
```

Metas razoáveis: **≥80% no domínio, ≥70% global**. Persegua **testes significativos**, não porcentagem.

### 4.9. Checklist de testes no CI

- [ ] `go test -race ./...` passa
- [ ] `go test -tags=integration ./...` passa (com Docker disponível)
- [ ] Cobertura de domínio ≥80%
- [ ] `go vet ./...` sem alertas
- [ ] Fuzz tests rodam por tempo fixo em nightly
- [ ] Benchmarks críticos comparados com `benchstat` em PRs de performance

### 4.10. Resumo de ferramentas de teste

| Categoria | Ferramenta | Quando usar |
|---|---|---|
| Framework base | `testing` (stdlib) | Sempre |
| Asserções | `testify/assert+require` **ou** `matryer/is` | Padronize uma |
| Mocks | `uber-go/mock` ou `mockery` | Interfaces grandes; prefira fakes manuais antes |
| HTTP handlers | `httptest` (stdlib) | Sempre |
| Integração com infra | `testcontainers-go` | Postgres, Kafka, Redis, etc. |
| Organização | subtests nativos; `testify/suite` se precisar | Default: nativos |
| Golden files | `sebdah/goldie` | Respostas grandes estáveis |
| Contract testing | `pact-go` | Contratos entre microsserviços |
| Fuzz | `testing.F` (stdlib) | Parsers, validadores |
| Benchmarks | `testing.B` + `benchstat` | Código hot-path |
| Property-based | `leanovate/gopter` | Domínio matemático |
| BDD | `godog` (Cucumber) | Só se QA escreve Gherkin |

---

## 🏗️ 5. Estrutura e Boas Práticas de Projeto

### 5.1. Layout

Inspirado em [`golang-standards/project-layout`](https://github.com/golang-standards/project-layout), **adaptado ao tamanho**:

```
meu-servico/
├── cmd/
│   └── api/                  # um main.go por binário
├── internal/                 # código privado (bloqueado pelo compilador)
│   ├── domain/               # entidades, VOs, regras puras
│   ├── application/          # use cases
│   ├── infrastructure/       # HTTP, DB, messaging
│   └── platform/             # logger, config, telemetry
├── pkg/                      # apenas se for biblioteca pública
├── api/                      # OpenAPI, proto, schemas
├── migrations/               # SQL migrations
├── deployments/              # Dockerfile, k8s
├── go.mod
└── go.sum
```

Regras: tudo em `internal/` por padrão; `main.go` é o **único** ponto de *wiring*.

### 5.2. Configuração (12-Factor)

- Env vars como fonte primária. Libs: [`caarlos0/env`](https://github.com/caarlos0/env) (declarativo) ou [`spf13/viper`](https://github.com/spf13/viper) (multi-fonte).
- Segredos via Vault / Secrets Manager — **nunca no repo**.
- Valide config no *startup*. **Fail-fast.**

### 5.3. Context e graceful shutdown

- Todo I/O recebe `context.Context` como 1º parâmetro.
- Capture `SIGTERM`/`SIGINT` e drene conexões:

```go
ctx, stop := signal.NotifyContext(context.Background(), os.Interrupt, syscall.SIGTERM)
defer stop()
// ... server.ListenAndServe em goroutine ...
<-ctx.Done()
shutdownCtx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
defer cancel()
server.Shutdown(shutdownCtx)
```

### 5.4. Erros

- Valores de primeira classe, sempre tratados.
- `fmt.Errorf("context: %w", err)` para envolver; `errors.Is`/`errors.As` para inspecionar.
- *Sentinel errors* no domínio (`var ErrUserNotFound = errors.New(...)`); mapeamento para HTTP status **em um só lugar**.

### 5.5. Observabilidade

- **Logs:** `log/slog` (stdlib, Go 1.21+), JSON estruturado, com `correlation_id` propagado via `context`.
- **Métricas:** `prometheus/client_golang` — métricas RED (Rate, Errors, Duration).
- **Tracing:** OpenTelemetry (`go.opentelemetry.io/otel`).
- **Health:** `/healthz` (liveness) e `/readyz` (readiness).

### 5.6. Qualidade no CI

Obrigatórios:

- `gofmt -l .` (falha se houver diff)
- `go vet ./...`
- [`golangci-lint`](https://golangci-lint.run/) com `errcheck`, `gocritic`, `revive`, `gosec`, `staticcheck`.
- `go test -race ./...`
- [`govulncheck`](https://pkg.go.dev/golang.org/x/vuln/cmd/govulncheck) contra CVEs.

### 5.7. Build e deploy

- Dockerfile *multi-stage* com imagem final `distroless` ou `scratch`.
- `CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w"` → binário estático < 25 MB.
- Respeite `SIGTERM` no orquestrador.

### 5.8. Documentação

- README com setup local em < 5 minutos.
- Contratos HTTP: OpenAPI + [`oapi-codegen`](https://github.com/deepmap/oapi-codegen) (gera client e server a partir do YAML).
- ADRs (*Architecture Decision Records*) em `docs/adr/`.

---

## 🎯 6. Clean Architecture e Clean Code em Go

### 6.1. Camadas

```
┌──────────────────────────────────────────────────┐
│  infrastructure/  HTTP, DB, Kafka, gRPC clients  │  adaptadores
├──────────────────────────────────────────────────┤
│  application/     Use Cases                      │  orquestração
├──────────────────────────────────────────────────┤
│  domain/          Entidades, VOs, regras         │  núcleo puro
└──────────────────────────────────────────────────┘
        Dependência sempre aponta para DENTRO
```

- `domain` importa só stdlib — zero HTTP, SQL, Kafka.
- `application` define interfaces (portas); `infrastructure` as implementa.
- DI manual no `main.go` ou via `google/wire` (geração de código, sem reflection).

### 6.2. Clean Code — idiomática primeiro

> Em Go, **idiomática > purismo de Clean Code**. Nem todo conselho do Uncle Bob se aplica.

- Nomes **curtos em escopo curto** (`i`, `ctx`, `err` são corretos — renomear seria ruim).
- Pacotes: `user`, `payment` (nunca `user_service`, `userservice`).
- Funções: um nível de abstração; retorno cedo; sem `else` após `return`.
- **Composição > Herança** (Go não oferece herança — caminho certo por default).
- **Interfaces pequenas**: *"The bigger the interface, the weaker the abstraction"*. `io.Reader` tem um método.
- **Sem getters/setters por default**; campos públicos quando não houver lógica.

---

## 🌐 7. Padrões para Microsserviços/BFF

| Padrão | Quando | Biblioteca |
|---|---|---|
| **Circuit Breaker** | Proteger contra falhas em cascata | [`sony/gobreaker`](https://github.com/sony/gobreaker) |
| **Retry com backoff** | Falhas transientes | [`avast/retry-go`](https://github.com/avast/retry-go) |
| **Bulkhead** | Isolar pools por dependência | `errgroup` + semáforos |
| **Rate limiting** | Proteger o serviço e clientes | [`x/time/rate`](https://pkg.go.dev/golang.org/x/time/rate) |
| **Idempotency key** | Escritas em APIs públicas | Manual + Redis/DB |
| **Outbox** | Consistência DB ↔ broker | Watermill built-in |
| **Saga** | Transações distribuídas | Watermill + orquestrador |
| **CQRS** | Leitura/escrita divergentes | Watermill-CQRS |
| **Cache-aside** | Reduzir latência | `go-redis/redis` |
| **Graceful degradation** | BFF com múltiplos upstreams | `errgroup` + fallbacks |

### 7.1. BFF — design específico

- **Agregue**, não apenas faça proxy.
- Chame upstreams **em paralelo** com `errgroup.WithContext` — BFF é I/O-bound.
- Timeout agressivo por upstream (`context.WithTimeout`); o BFF tem seu próprio SLA.
- Schema adaptado ao cliente (mobile ≠ web). Cada BFF serve um canal.
- Esconda falhas parciais; sinalize degradação no payload.

### 7.2. DDD leve

- Linguagem ubíqua nos nomes (tipos e pacotes).
- Agregados como unidades de transação (1 aggregate = 1 transação).
- Repository pattern: interface no `application`, implementação no `infrastructure`.

### 7.3. Referências de design

- 📘 *Designing Data-Intensive Applications* — Kleppmann (obrigatório).
- 📘 *Building Microservices* — Sam Newman.
- 🌐 [microservices.io](https://microservices.io/) — catálogo de padrões.
- 🌐 [threedots.tech](https://threedots.tech/) — Go + DDD + Clean Arch aplicados.

---

## ✅ Checklist de Produção

- [ ] `go test -race ./...` verde em CI
- [ ] `go test -tags=integration ./...` verde em CI (com Docker)
- [ ] `golangci-lint run` sem erros
- [ ] `govulncheck ./...` sem CVEs abertas
- [ ] Cobertura ≥80% em domínio, ≥70% global
- [ ] `/healthz` e `/readyz` respondendo
- [ ] Logs JSON com `correlation_id`
- [ ] Métricas Prometheus em `/metrics`
- [ ] Traces OpenTelemetry emitidos
- [ ] Graceful shutdown testado
- [ ] Dockerfile *multi-stage* com `distroless`/`scratch`, < 25 MB
- [ ] `context.Context` em 100% do I/O
- [ ] Timeouts em todo cliente externo
- [ ] Rate limiting nos endpoints públicos
- [ ] OpenAPI publicado
- [ ] README com setup em < 5 min
- [ ] ADRs das decisões principais

---

## 📌 Nota Final

A transição de Kotlin para Go **não é tradução de sintaxe** — é mudança de filosofia. Go recompensa:

- **Simplicidade explícita** sobre abstrações elegantes.
- **Composição** sobre hierarquia.
- **Erros como valores** sobre exceções.
- **Código repetitivo e claro** sobre DSLs *clever*.

Use este documento como mapa. A cada sprint, revisite a trilha: **em que fase estou? o que falta para o próximo checkpoint?**

Boa jornada. 🦫
