# Helios Voting – Sistema de Eleições Verificáveis

Helios é um sistema de votação online com foco em **verificabilidade de ponta a ponta** (“end‑to‑end verifiable voting”).  
Em termos simples, ele permite que eleições sejam realizadas pela internet, garantindo:

- que cada eleitor possa conferir se seu voto foi realmente contabilizado;
- que ninguém consiga descobrir em quem você votou;
- que a comunidade possa auditar a eleição sem depender apenas da confiança na equipe técnica.

Este repositório corresponde à implementação original do **Helios Election System**, desenvolvida em contexto acadêmico e de pesquisa, e hoje amplamente usada como **referência teórica e prática** em segurança de votação eletrônica.

---

## Origem acadêmica e objetivos

O Helios foi criado como um projeto de pesquisa em **criptografia aplicada a eleições**.  
Alguns objetivos centrais:

- demonstrar, na prática, que é possível ter eleições online:
  - com **sigilo de voto** (ninguém sabe em quem você votou);
  - com **verificação independente** (o eleitor e observadores conseguem checar a apuração);
- servir de base para estudos acadêmicos, artigos científicos e experimentos em eleições reais;
- mostrar que um sistema de votação pode ser **transparente, auditável e aberto** (código livre).

Do ponto de vista acadêmico, o Helios ficou conhecido por:

- utilizar criptografia homomórfica para apurar votos sem revelar votos individuais;
- permitir que cada eleitor receba um **“ballot tracker”** (código de rastreamento da cédula);
- disponibilizar todos os dados necessários para auditoria pública do resultado.

---

## Papel neste projeto (UFSC / e‑democracia)

Este código serviu como **base conceitual e técnica** para o desenvolvimento de uma nova versão do Helios Voting, adaptada para o contexto de eleições da **Universidade Federal de Santa Catarina (UFSC)**, no âmbito do projeto de **e‑democracia**.

Em linhas gerais, a partir desta versão original foram feitos, em outro repositório:

- adaptações de interface (tradução para português, ajustes de fluxo, rótulos, mensagens);
- adequações a requisitos institucionais da UFSC (processo eleitoral interno, papéis, fluxos);
- integrações com a infraestrutura local (autenticação, ambiente de execução, monitoramento);
- melhorias de usabilidade para eleitores, administradores e fiscais de eleição.

Ou seja:

- **este repositório** representa o Helios original, como referência acadêmica e técnica;
- **a nova versão** (utilizada no projeto de e‑democracia da UFSC) é uma derivação/customização
  que herda os conceitos de verificabilidade, sigilo e auditabilidade do Helios, mas ajustada
  à realidade institucional brasileira e aos processos da UFSC.

---

## O que o Helios faz, em termos práticos

De forma não técnica, o Helios oferece:

- **Criação de eleições**
  - Cadastro de uma nova eleição, com título, descrição e datas de início/fim da votação.
  - Configuração de quem pode votar (lista fechada de eleitores, grupos, eleições abertas, etc.).
  - Definição de perguntas e opções de voto (candidatos, chapas, alternativas).

- **Cabine de votação online**
  - Cada eleitor acessa uma cabine virtual, seleciona suas opções e confirma o voto.
  - Antes de enviar definitivamente, o voto é **criptografado no navegador** do eleitor.
  - O servidor recebe somente o voto criptografado, sem saber o conteúdo.

- **Rastreamento de cédula**
  - Ao final, o eleitor recebe um **código de rastreamento** (ballot tracker).
  - Esse código permite verificar, em uma lista pública de cédulas, que o voto entrou na apuração.
  - Isso é feito sem revelar o conteúdo do voto.

- **Apuração verificável**
  - Todos os votos criptografados são combinados em uma **apuração criptografada**.
  - Um conjunto de responsáveis (trustees) colabora na **descriptografia apenas do resultado final**, não dos votos individuais.
  - Qualquer pessoa pode conferir, matematicamente, que o resultado divulgado é compatível com os votos publicados (sem saber em quem cada um votou).

- **Auditoria**
  - Eleitores e observadores podem fazer verificações independentes:
    - conferir se o código de rastreamento aparece na lista de cédulas;
    - verificar provas criptográficas associadas à apuração;
    - verificar, com ferramentas auxiliares, se o resultado está consistente com os dados publicados.

---

## Quem costuma usar o Helios

Além de UFSC e de usos acadêmicos, o Helios é tradicionalmente utilizado para:

- eleições de **associações, conselhos, ordens profissionais** e entidades estudantis;
- votações de **departamentos universitários, programas de pós‑graduação e colegiados**;
- consultas internas e plebiscitos em organizações que desejam transparência e auditabilidade.

O foco do Helios é oferecer um sistema adequado para cenários em que:

- há interesse em **transparência** e **verificação independente**;
- a eleição não está sujeita ao mesmo nível de ataque, coerção ou fraude de uma eleição nacional;
- a instituição está disposta a seguir boas práticas de operação, divulgação e auditoria.

---

## Aviso importante

Embora seja robusto e amplamente estudado, o Helios:

- **não foi desenhado** para substituir eleições oficiais de grande escala (como eleições gerais nacionais);
- exige um mínimo de cuidado operacional (gestão de chaves, configuração correta, comunicação clara com eleitores);
- deve ser implantado em ambientes que entendam e valorizem o modelo de verificabilidade que ele oferece.

A versão utilizada no projeto de **e‑democracia da UFSC** segue essas premissas, aproveitando a base
acadêmica do Helios para construir um sistema ajustado à realidade da universidade, com foco em
**eleições internas transparentes e auditáveis**, **participação eletrônica segura** e
**fortalecimento da confiança** nos processos de decisão da comunidade acadêmica.

