# Banco API Tests

Projeto de automação de testes de API REST desenvolvido em JavaScript para validar os principais fluxos da **Banco API**, incluindo autenticação, consulta e realização de transferências.

**Repositório dos testes:** [GabrielToldato/banco-api-tests](https://github.com/GabrielToldato/banco-api-tests)  
**API sob teste:** [GabrielToldato/banco-api](https://github.com/GabrielToldato/banco-api)

## Objetivo

O objetivo deste projeto é automatizar verificações de endpoints REST, validando códigos de status HTTP, conteúdo e tipos dos dados retornados pela API.

Os cenários presentes no projeto incluem:

- Autenticação de usuário e obtenção de token;
- Criação de transferência com valor permitido;
- Validação de transferência abaixo do valor mínimo;
- Consulta de transferência por ID;
- Consulta de transferências com paginação.

Os testes utilizam autenticação via token Bearer nos endpoints protegidos.

## Stack e ferramentas

| Tecnologia | Utilização |
|---|---|
| JavaScript (Node.js) | Linguagem e ambiente de execução dos testes |
| Mocha | Organização e execução dos casos de teste |
| Supertest | Envio de requisições HTTP para a API |
| Chai | Assertions e validação dos resultados |
| dotenv | Carregamento das variáveis definidas no arquivo `.env` |
| Mochawesome | Geração de relatório HTML dos testes |

As versões das dependências são controladas pelo `package.json` e pelo `package-lock.json`.

## Estrutura de diretórios

```text
banco-api-tests/
├── fixtures/
│   ├── postLogin.json
│   └── postTransferencias.json
├── helpers/
│   └── autenticacao.js
├── test/
│   └── arquivos de testes automatizados (*.test.js)
├── .env                 # Criado localmente; não deve ser versionado
├── .gitignore
├── package.json
└── package-lock.json
```

### Responsabilidade de cada diretório

- **`fixtures/`**: contém arquivos JSON com dados utilizados nas requisições, como credenciais de login e dados de transferência.
- **`helpers/`**: reúne funções auxiliares reutilizáveis. O helper `autenticacao.js`, por exemplo, realiza o login e retorna o token recebido.
- **`test/`**: contém os cenários automatizados. Os arquivos seguem o padrão `*.test.js`, utilizado pelo comando configurado no Mocha.
- **`.env`**: armazena configurações locais do ambiente, como a URL base da API.
- **`package.json`**: define as dependências e os scripts disponíveis para execução.

## Pré-requisitos

Antes de executar os testes, instale:

1. [Node.js](https://nodejs.org/) (versão compatível com as dependências do projeto; recomenda-se uma versão LTS atual).
2. npm, instalado junto com o Node.js.
3. A Banco API em execução e acessível pela máquina que executará os testes.

> A API deve estar inicializada antes da execução da suíte. Os testes também dependem dos usuários e registros de teste esperados pelos cenários.

## Configuração do ambiente

### 1. Clonar o repositório

```bash
git clone https://github.com/GabrielToldato/banco-api-tests.git
cd banco-api-tests
```

### 2. Instalar as dependências

```bash
npm install
```

O comando instala as bibliotecas declaradas no `package.json`, respeitando o arquivo `package-lock.json`.

### 3. Criar o arquivo `.env`

O arquivo `.env` não é incluído no repositório e precisa ser criado localmente, na raiz do projeto — no mesmo nível do `package.json`.

Adicione a variável `BASE_URL` com o endereço raiz da API:

```env
BASE_URL=http://localhost:3000
```

Ajuste o endereço e a porta de acordo com a configuração da API em execução. Não coloque aspas nem espaços ao redor do sinal de igualdade.

A variável é carregada pelo `dotenv` e utilizada pelo Supertest para definir a URL base das requisições.

**Importante:** não envie o arquivo `.env` para o GitHub. Ele está listado no `.gitignore`. Não armazene senhas, tokens reais ou outras informações sensíveis no repositório.

## Executando os testes

Com a API em execução e o `.env` configurado, execute na raiz do projeto:

```bash
npm test
```

O script definido no `package.json` executa:

```bash
mocha ./test/**/*.test.js --timeout=200000 --reporter mochawesome
```

Isso significa que:

- O Mocha procura arquivos de teste com extensão `.test.js` dentro de `test/`;
- O timeout configurado para cada teste é de 200.000 milissegundos (200 segundos);
- O Mochawesome é utilizado como reporter para gerar o relatório.

## Relatório HTML com Mochawesome

Ao executar `npm test`, o Mochawesome gera o relatório HTML e os arquivos de apoio no diretório padrão:

```text
mochawesome-report/
```

Abra o arquivo `mochawesome.html` em um navegador para visualizar o resultado da execução, incluindo os testes aprovados, reprovados e detalhes dos erros.

> O diretório de saída padrão do reporter é `mochawesome-report/` (com hífen), não `mochawesome/`. O diretório e os arquivos gerados são resultados da execução e não precisam ser criados manualmente.

### Resumo dos comandos

| Ação | Comando |
|---|---|
| Instalar dependências | `npm install` |
| Executar todos os testes | `npm test` |
| Gerar o relatório HTML | `npm test` |
| Abrir o relatório | Abrir `mochawesome-report/mochawesome.html` no navegador |

## Observações importantes

- A API precisa estar disponível no endereço configurado em `BASE_URL`.
- Os cenários utilizam dados de teste definidos em `fixtures/` e podem depender da existência de usuários, contas e transferências específicas na base de dados da API.
- Caso a API utilize outra porta ou host, atualize o valor de `BASE_URL`.
- Verifique se todas as requisições utilizam a variável `BASE_URL`. No código atual, há requisições em um cenário de transferência que utilizam diretamente `http://localhost:3000`; se o endereço da API for diferente, ajuste esse trecho para utilizar `process.env.BASE_URL` também.
- O projeto de testes e a API são repositórios separados. Inicie a API em um terminal e execute a suíte de testes em outro.

## Documentação oficial das dependências

- [Node.js](https://nodejs.org/docs/latest/api/)
- [Mocha](https://mochajs.org/)
- [Supertest](https://github.com/ladjs/supertest)
- [Chai](https://www.chaijs.com/)
- [dotenv](https://github.com/motdotla/dotenv)
- [Mochawesome](https://github.com/adamgruber/mochawesome)
- [npm](https://docs.npmjs.com/)

## Autor

**Gabriel Teles Toldato**

Projeto desenvolvido para estudo e prática de automação de testes de APIs REST, com foco em qualidade de software e validação de serviços HTTP.
