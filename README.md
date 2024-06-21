**=======================AUTOMAÇÃO - TESTES DE API=======================**

# Descrição do Projeto:

O presente projeto tem uma suite de teste desenvolvido com a ferramenta Cypress, utilizando NodeJS e linguagem JS.
Tem a finalidade de automatizar cenários de testes da API https://viacep.com.br/ . Está com o formato de Custom Command
afim de melhorar o entendimento do código e diminuir repetições, deixando o código mais limpo.

## Instalação:

- Ter o NodeJS instalado na máquina;
- Instalar dependências que já se encontram no package.json, rodar comando(npm install);
- Executar o cypress(npx cypress open), ou executar apenas os testes e obter relatório no terminal, rodar o comando(npx cypress run).

### Estrutura do Projeto:

- **cypress**: Pasta onde se encontra o projeto Cypress;
- **cypress/e2e**: Pasta onde se encontra o arquivo de testes;
- **cypress/support**: Pasta onde se encontra os arquivos de suporte;
- **cypress/support/commands.js**: Arquivo onde se encontram os Custom Commands;

#### Explicando alguns métodos e funções:

-Custom Commandas:
**cy.validateValidCepResponse()** - valida a resposta quando enviado um cep válido
**cy.validateInvalidCepResponse()** - valida a resposta quando enviado um cep inválido
**cy.requestValidFormatCep()** - Faz a requisição com um cep válido
**cy.requestInvalidFormatCep()** - Faz uma requisição com cep inválido

-Nativo
**failOnStatusCode: false** - Evita que o cypress quebre a aplicação para que possamos validar os status e body nos cenários de erro.

#### Cobertura dos Testes:

Feature: Consultar/Validar CEP

Cenário 1: Consultar CEP válido
Dado que o CEP "01001-000" é válido
Quando eu envio uma requisição para a API de consulta de CEP com o CEP "01001-000"
Então a resposta deve ter o status code 200
E o status ser existente

Cenário 2: Valida retorno JSON para consulta válida
Dado que o CEP "01001-000" é válido
Quando eu envio uma requisição para a API de consulta de CEP com o CEP "01001-000"
Então retorna um body contendo "cep", "logradouro", "complemento", "bairro", "localidade", "uf", "ibge", "gia", "ddd", "siafi"

Cenário 3: Consulta sem fornecer o tipo do retorno
Dado que o CEP "01001-000" é válido
Quando eu envio uma requisição para a API de consulta de CEP com o CEP "01001-000" sem o retorno esperado(json)
Então a resposta deve ter o status code 400
E o status ser existente

Cenário 4: Consultar CEP inválido - 9 digitos
Dado que o CEP "950100100" é inválido
Quando eu envio uma requisição para a API de consulta de CEP com o CEP "950100100"
Então a resposta deve ter o status code 400
E o status ser existente

Cenário 5: Consultar CEP inválido - alfanuméricos
Dado que o CEP "95010A10" é inválido
Quando eu envio uma requisição para a API de consulta de CEP com o CEP "95010A10"
Então a resposta deve ter o status code 400
E o status ser existente

Cenário 6: Consultar CEP inválido - com espaço
Dado que o CEP "95010 10" é inválido
Quando eu envio uma requisição para a API de consulta de CEP com o CEP "95010 10"
Então a resposta deve ter o status code 400
E o status ser existente

Cenário 7: Consultar CEP inválido - 7 digitos
Dado que o CEP "0100100" é inválido
Quando eu envio uma requisição para a API de consulta de CEP com o CEP "0100100"
Então a resposta deve ter o status code 400
E o status ser existente

Cenário 8: Consultar CEP inválido - apenas letras
Dado que o CEP "ABCDEFGH" é inválido
Quando eu envio uma requisição para a API de consulta de CEP com o CEP "ABCDEFGH"
Então a resposta deve ter o status code 400
E o status ser existente

Cenário 9: Consultar CEP inválido - campo vazio(sem valor)
Dado que o CEP " " é inválido
Quando eu envio uma requisição para a API de consulta de CEP com o CEP " "
Então a resposta deve ter o status code 400
E o status ser existente

Cenário 10: Consulta de CEP inexistente de formato válido - 8 digitos
Dado que o CEP "99999999" é inexistente
Quando eu envio uma requisição para a API de consulta de CEP com o CEP "99999999"
Então a resposta deve ter o status code 200
E body "erro": true
