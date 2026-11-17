# GlobalExpress — estrutura inicial v0.4

Projeto Spring Boot / Java 21 com módulos básicos de clientes e carteiras.

## Endpoints
- `POST /api/v1/customers` — regista um cliente com estado inicial `PENDING`.
- `POST /api/v1/customers/{customerId}/wallet` — cria a carteira e as seis contas; se já existir, devolve a existente.
- `GET /api/v1/customers/{customerId}/wallet` — consulta a carteira.

## Carteiras e moedas
- Uma carteira por cliente.
- Seis contas de moeda por carteira: AOA, EUR, USD, GBP, CHF e BRL.
- Uma conta por moeda, protegida por restrição única na base de dados.
- Não existe saldo armazenado nesta fase. Os saldos serão derivados do livro-razão financeiro; não se deve editar um saldo diretamente.

## Base de dados
Configure `DB_URL`, `DB_USERNAME` e `DB_PASSWORD`. As migrações Flyway V1 (clientes) e V2 (carteiras) criam as tabelas. Hibernate está em modo `validate` e não cria nem altera tabelas automaticamente.

## Limitações
Esta versão é uma base de desenvolvimento, não um serviço financeiro pronto para produção. Não inclui ainda autenticação, KYC executável, livro-razão integrado, saldos, transferências reais, integrações bancárias, câmbio, licenças ou controlos completos de concorrência. É necessário compilar e executar testes num ambiente com Maven e PostgreSQL antes de considerar o código validado.


## Livro-razão inicial (v0.5)

A migração `V3__create_ledger.sql` cria `ledger_transactions` e `ledger_entries`. O serviço interno `LedgerPostingService` regista um movimento equilibrado com um débito e um crédito de igual valor, exige contas da mesma moeda e impede valores não positivos. Cada referência de operação é única.

**Limitações importantes:** esta base é apenas a fundação contabilística. Ainda não existe endpoint público para movimentar dinheiro, cálculo de saldo disponível, bloqueio concorrente de saldo, prevenção completa de saldo negativo, conversão cambial, integração com bancos/parceiros, autorização de transferências, nem validação de produção. O serviço não deve ser exposto ao cliente nem usado para movimentar fundos reais antes dessas proteções, testes e aprovações.
