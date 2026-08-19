# pc-lopal
Repositório para armazenar os códigos da aula.

d 1
Create a markdown file containing the explanation of Semantic Versioning formatted properly.
markdown_content = """# Entendendo o Versionamento Semântico (SemVer) em Bibliotecas JavaScript

Toda biblioteca JavaScript utiliza um padrão de numeração no formato MAJOR.MINOR.PATCH (por exemplo, 1.0.0). Esse padrão é conhecido como Semantic Versioning (Versionamento Semântico) e serve para comunicar de forma clara o tipo de alteração feita em cada atualização do código.


O que significa cada um dos três números?
A estrutura é dividida em três partes principais:
1. MAJOR (Versão Principal) — Ex: 1.0.0 $\rightarrow$ 2.0.0
O que indica: Mudanças incompatíveis com versões anteriores (breaking changes).
Impacto: Funções, sintaxes ou comportamentos antigos podem ter sido alterados ou removidos.
Ação recomendada: Atualizar exige revisão no código existente, pois a aplicação pode quebrar se não forem feitos os ajustes necessários.
2. MINOR (Versão Secundária) — Ex: 1.0.0 $\rightarrow$ 1.1.0
O que indica: Adição de novas funcionalidades de forma retrocompatível (backwards-compatible).
Impacto: Novos recursos e APIs são disponibilizados, mas tudo o que já existia continua funcionando exatamente do mesmo jeito.
Ação recomendada: É seguro atualizar; o código existente não será afetado.
3. PATCH (Correção) — Ex: 1.0.0 $\rightarrow$ 1.0.1
O que indica: Correções de bugs, pequenas otimizações de desempenho ou patches de segurança.
Impacto: Nenhuma funcionalidade nova é adicionada e nenhuma interface existente é alterada.
Ação recomendada: Atualização altamente recomendada e segura para manter a estabilidade da aplicação.


Quem decide como esse número muda e com base em quê?
Quem decide?
A decisão é tomada exclusivamente pelos mantenedores e desenvolvedores responsáveis pelo projeto/biblioteca.
Com base em quê?
Os mantenedores avaliam a natureza e o impacto das alterações realizadas no código antes de publicar a nova versão no gerenciador de pacotes (como o npm):

Quebrou o código existente de quem consome a biblioteca?
$\rightarrow$ Incrementa o MAJOR e reseta MINOR e PATCH para zero (1.4.2 $\rightarrow$ 2.0.0).
Adicionou novos recursos sem quebrar nada?
$\rightarrow$ Incrementa o MINOR e reseta o PATCH para zero (1.4.2 $\rightarrow$ 1.5.0).
Apenas corrigiu um erro sem mudar a forma de uso?
$\rightarrow$ Incrementa o PATCH (1.4.2 $\rightarrow$ 1.4.3).



Resumo: Seguir a especificação do SemVer funciona como um contrato de confiança entre os criadores da biblioteca e os desenvolvedores que a utilizam, garantindo atualizações mais previsíveis e automações seguras em ferramentas de gerenciamento de dependências. """

file_path = "versionamento_semantico.md" with open(file_path, "w", encoding="utf-8") as f: f.write(markdown_content)

print(f"File created: {file_path}")


d 2 
Diferença entre dependencies e devDependencies no package.json
No arquivo package.json, as bibliotecas de um projeto Node.js/JavaScript são divididas em diferentes seções. As duas principais são dependencies e devDependencies.


1. Qual a diferença entre os dois grupos?
dependencies (Dependências de Produção)
Definição: Bibliotecas indispensáveis para o funcionamento da aplicação em ambiente de execução (runtime).
Ambiente: Executadas em produção (no servidor, no navegador do usuário final ou no aplicativo).
Impacto se ausente: A aplicação falhará ou quebrará em tempo de execução para os usuários.
Exemplos:
react / vue / angular (frameworks de interface)
express / fastify (frameworks web/backend)
axios (cliente HTTP)
lodash (biblioteca de utilitários)
devDependencies (Dependências de Desenvolvimento)
Definição: Ferramentas auxiliares necessárias apenas durante o processo de desenvolvimento, construção, testes e manutenção do projeto.
Ambiente: Executadas apenas na máquina do desenvolvedor ou em pipelines de integração contínua (CI/CD).
Impacto se ausente: A aplicação em produção continua funcionando perfeitamente, mas os desenvolvedores não conseguirão testar, compilar ou formatar o código localmente.
Exemplos:
jest / vitest / cypress (frameworks de teste)
eslint / prettier (linters e formatadores)
typescript (compilador)
vite / webpack (empacotadores/build tools)


2. Como decidir em qual grupo colocar uma biblioteca?
Para decidir onde incluir cada biblioteca, você pode se fazer a seguinte pergunta chave:

"A aplicação precisa dessa biblioteca para rodar enquanto o usuário final estiver utilizando o sistema?"

SIM: Instale em dependencies

npm install <nome-da-biblioteca>
# ou
yarn add <nome-da-biblioteca>

(O pacote fará parte do pacote final distribuído/executado em produção).

NÃO: Instale em devDependencies

npm install --save-dev <nome-da-biblioteca>
# ou
npm i -D <nome-da-biblioteca>

(O pacote será ignorado ao rodar npm install --production).


Tabela Comparativa
Critério
dependencies
devDependencies
Necessário em produção?
Sim
Não
Público-alvo
Usuário final / Servidor de produção
Desenvolvedor / Pipeline de CI/CD
Exemplo prático
Criar rotas no servidor (express)
Rodar testes automatizados (jest)
Comando de instalação
npm i <pacote>
npm i -D <pacote>


d 3
O que significam os símbolos ^, ~ e versões exatas no package.json
No arquivo package.json, esses caracteres são chamados de operadores de intervalo de versão (version range operators). Eles instruem o gerenciador de pacotes (como o npm ou yarn) sobre quais atualizações automáticas são permitidas ao rodar comandos como npm update ou ao instalar o projeto em uma nova máquina.


1. O que cada símbolo permite atualizar?
^ (Circunflexo / Caret) — Atualizações de Recursos e Correções
O que permite: Permite atualizar as versões MINOR (recursos novos) e PATCH (correções de bugs), mas bloqueia qualquer mudança na versão MAJOR (quebras de compatibilidade).
Objetivo: Maximizar o recebimento de novidades e melhorias com baixo risco de quebrar a aplicação.
Exemplo (^1.2.3):
Permite: 1.2.4, 1.3.0, 1.9.9.
Bloqueia: 2.0.0 em diante.
Nota sobre a versão 0.x.x: Como versões iniciais (0.1.0, 0.2.0, etc.) são consideradas instáveis no SemVer, o operador ^ em ^0.2.3 atualizará apenas o PATCH (bloqueando 0.3.0).
~ (Tio / Tilde) — Apenas Correções de Bugs
O que permite: Permite atualizar exclusivamente a versão PATCH (correções de bugs e segurança), bloqueando mudanças em MINOR e MAJOR.
Objetivo: Priorizar a estabilidade do sistema, recebendo apenas correções urgentes e sem adicionar novas funcionalidades.
Exemplo (~1.2.3):
Permite: 1.2.4, 1.2.9.
Bloqueia: 1.3.0 e 2.0.0.


2. O que acontece quando não existe nenhum símbolo?
Versão Exata (Exact Version)
Quando uma versão é declarada sem nenhum caractere prefixado (por exemplo, "1.2.3"):

O que acontece: O gerenciador de pacotes trava a instalação exatamente nessa versão.
Comportamento: Nenhuma atualização será baixada automaticamente — nem mesmo correções de segurança ou pequenos reparos de erros.
Quando utilizar:
Em projetos críticos onde a estabilidade precisa ser 100% garantida.
Para evitar regressões (quando a atualização de um bug acidentalmente insere outro problema no código).


Tabela Comparativa
Símbolo
Exemplo
Tipo de Atualização Permitida
Intervalo Aceito
^ (Circunflexo)
^1.4.2
Minor e Patch
>= 1.4.2 e < 2.0.0
~ (Tio)
~1.4.2
Apenas Patch
>= 1.4.2 e < 1.5.0
Nenhum
1.4.2
Nenhuma (Versão Fixa)
Apenas 1.4.2




Curiosidade: Por padrão, quando você roda npm install <nome-da-lib>, o npm adiciona automaticamente o operador ^ antes do número da versão no seu package.json.

d 4
Guia de Estudos: Gerenciamento de Módulos e Dependências em JavaScript

Atividade 1: Versionamento Semântico (SemVer)
Toda biblioteca JavaScript utiliza um padrão de numeração no formato MAJOR.MINOR.PATCH (por exemplo, 1.0.0). Esse padrão é conhecido como Semantic Versioning (Versionamento Semântico) e comunica o tipo de alteração feita em cada atualização.
1. O que significa cada um dos três números?
1º Número — MAJOR (Versão Principal): Muda quando ocorrem alterações incompatíveis (breaking changes). Funções antigas podem ter sido removidas ou modificadas de forma que seu código atual pode parar de funcionar se você atualizar sem fazer ajustes.
2º Número — MINOR (Versão Secundária): Muda quando novas funcionalidades são adicionadas de forma retrocompatível (backwards-compatible). Você ganha novos recursos e seu código antigo continua funcionando normalmente.
3º Número — PATCH (Correção): Muda para correções de bugs ou pequenas melhorias internas. É uma atualização segura que repara falhas sem alterar a forma como a biblioteca é utilizada.
2. Quem decide como esse número muda e com base em quê?
Quem decide: Os mantenedores e desenvolvedores responsáveis pelo projeto.
Com base em quê: Na natureza e no impacto das alterações no código:
Exige que os usuários alterem seu código? $\rightarrow$ Incrementa MAJOR (1.8.4 $\rightarrow$ 2.0.0).
Criou uma função nova sem quebrar o existente? $\rightarrow$ Incrementa MINOR (1.8.4 $\rightarrow$ 1.9.0).
Foi apenas um ajuste de erro ou segurança? $\rightarrow$ Incrementa PATCH (1.8.4 $\rightarrow$ 1.8.5).


Atividade 2: dependencies vs devDependencies no package.json
No arquivo package.json, as bibliotecas de um projeto são divididas em diferentes seções de acordo com a necessidade de execução.
1. Qual a diferença entre os dois grupos?
dependencies (Dependências de Produção): Bibliotecas indispensáveis para a aplicação funcionar em tempo de execução (runtime). São executadas em produção (servidor ou navegador do usuário final).
Exemplos: react, express, axios, lodash.
devDependencies (Dependências de Desenvolvimento): Ferramentas auxiliares necessárias apenas durante o desenvolvimento, compilação, testes e manutenção do código. Não são enviadas para o ambiente final de produção.
Exemplos: jest, eslint, typescript, vite, prettier.
2. Como decidir em qual grupo colocar uma biblioteca?
Faça a seguinte pergunta:

"A aplicação precisa dessa biblioteca para rodar enquanto o usuário final estiver utilizando o sistema?"

SIM: Instale em dependencies (npm install <nome-da-lib>).
NÃO: Instale em devDependencies (npm install -D <nome-da-lib>).


Atividade 3: Símbolos de Versionamento (^, ~ e Versão Exata)
No package.json, os símbolos prefixados no número da versão controlam as atualizações automáticas do gerenciador de pacotes (npm ou yarn).
1. O que cada símbolo permite atualizar?
^ (Circunflexo / Caret): Permite atualizar MINOR e PATCH, bloqueando mudanças na versão MAJOR.
Exemplo (^1.2.3): Aceita 1.2.4 e 1.3.0, mas bloqueia 2.0.0.
~ (Tio / Tilde): Permite atualizar exclusivamente a versão PATCH (correções de bugs e segurança).
Exemplo (~1.2.3): Aceita 1.2.4, mas bloqueia 1.3.0 e 2.0.0.
2. O que acontece quando não existe nenhum símbolo?
Versão Exata (Exact Version): O gerenciador de pacotes fixa a instalação exatamente naquela versão (ex: "1.2.3"). Nenhuma atualização automática (mesmo de correção de bug) será baixada. Isso garante 100% de previsibilidade e evita regressões.


Atividade 4: CommonJS vs ES Modules (ESM)
JavaScript possui dois sistemas de módulos principais para organizar e reaproveitar código entre arquivos.
1. Como cada um surgiu?
CommonJS (CJS): Criado em 2009 (ServerJS) para padronizar módulos de JS fora do navegador. Foi adotado pelo Node.js como o padrão original da plataforma.
ES Modules (ESM): Criado em 2015 na especificação ES6 (ECMAScript 2015) para ser o padrão oficial e nativo da linguagem JS, funcionando tanto em navegadores quanto no Node.js.
2. Qual a diferença entre os dois?
Característica
CommonJS (CJS)
ES Modules (ESM)
Padrão
Padrão do Node.js / comunidade
Padrão oficial do ECMAScript
Carregamento
Síncrono (em runtime)
Assíncrono (análise estática)
Suporte em Navegadores
Requer empacotador (Webpack)
Nativamente suportado

3. Sintaxe de Importação e Exportação
CommonJS (CJS)
// Exportando
module.exports = function soma(a, b) { return a + b; };
// ou
exports.soma = (a, b) => a + b;

// Importando
const soma = require('./matematica');
ES Modules (ESM)
// Exportando
export default function soma(a, b) { return a + b; }
// ou
export const soma = (a, b) => a + b;

// Importando
import soma from './matematica.js';
// ou
import { soma } from './matematica.js';
