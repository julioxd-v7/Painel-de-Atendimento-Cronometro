# Painel de Atendimento — Ti fy Soluções

Esse painel eu criei pra organizar o atendimento da nossa equipe de suporte e dar visibilidade real do que está acontecendo, sem precisar ficar perguntando no WhatsApp quem está fazendo o quê.

É um site único (um arquivo `index.html`), sem servidor próprio, hospedado no GitHub Pages. Os dados ficam salvos no Firebase Realtime Database, então tudo o que acontece aparece na hora em qualquer computador que estiver com o site aberto, seja o meu, do Julio, do Romulo ou de um supervisor.

## O que ele faz

**Login único com dois perfis.** O mesmo campo de usuário e senha reconhece se quem está entrando é do Suporte TI ou é um Supervisor, e abre a tela certa pra cada um.

**Cronômetro de serviços (lado do TI).** A gente cadastra os tipos de serviço que executa no dia a dia (Relatórios, Suporte técnico, etc), inicia e finaliza o cronômetro conforme vai atendendo, e isso fica registrado com data, hora, duração e quem atendeu.

**Chamados dos supervisores.** O supervisor abre um chamado dizendo o tipo de problema e uma observação, o TI recebe na hora, decide se aceita ou recusa (com motivo), coloca em atendimento, resolve e conclui registrando a solução. Se for recusado, o supervisor vê o motivo e pode reabrir.

**Categorias editáveis.** Os tipos de problema que aparecem como sugestão pro supervisor são cadastrados por mim ou por quem for do TI, então dá pra ir ajustando conforme os problemas mais comuns forem aparecendo.

**Chat.** Cada supervisor tem uma conversa direta com o Suporte TI, e o TI vê todas as conversas numa caixa de entrada só, como um chat de suporte mesmo.

**Equipe online.** Dá pra ver em tempo real quem está online, offline, e o que cada um está atendendo no momento, tanto do lado do TI quanto dos supervisores.

**Gestão de usuários pelo próprio site.** Não precisa mexer em código pra criar um login novo. Tem um botão de engrenagem no topo que abre o cadastro de usuários do TI e dos supervisores.

**Minha equipe.** Cada supervisor cadastra o nome de quem está sob sua equipe, pra poder vincular um chamado a uma pessoa específica quando for o caso.

**Relatórios.** Tem visão geral de tempo por serviço, quantidade e tempo médio de atendimento dos chamados, chamados por tipo, e exportação em Excel por dia, por período escolhido ou tudo de uma vez.

**Encerramento automático.** Se alguém fechar a aba, cair a conexão ou o computador desligar do nada, o atendimento em andamento se fecha sozinho, sem ficar contando tempo à toa.

## Como está montado por trás

Tudo roda num arquivo `index.html` só, com HTML, CSS e JavaScript puro, sem framework nenhum. A conexão com os dados é feita com o SDK do Firebase (`firebase-app-compat.js` e `firebase-database-compat.js`), carregado direto por link no próprio HTML.

A estrutura dos dados no Firebase é mais ou menos assim:

```
logins/ti/{usuario}         → senha dos usuários do Suporte TI
logins/sup/{usuario}        → senha dos supervisores
teams/{supervisor}/{id}     → nome dos integrantes da equipe de cada supervisor
categories/{id}             → tipos de problema cadastrados pelo TI
services/{id}                → serviços do cronômetro
active/{id}                  → serviços em atendimento agora
days/{data}/{id}             → histórico de atendimentos por dia
tickets/{id}                 → chamados abertos pelos supervisores
chats/{supervisor}/{id}      → mensagens trocadas com o TI
presence/{usuario}           → status online/offline de cada um
```

A exportação em Excel usa a biblioteca `xlsx.full.min.js`, também carregada por link, sem precisar instalar nada.

## Pra rodar

Não precisa de instalação. É só manter o arquivo `index.html` publicado no GitHub Pages com o Firebase configurado (a chave de config já está dentro do próprio arquivo, apontando pro projeto `painel-tifysolucoes`). Abrindo o link do GitHub Pages, o site já funciona direto.

Os primeiros logins do TI (`eber`, `julio`, `romulo`) são criados sozinhos no banco na primeira vez que alguém abre o site com o Firebase vazio. Supervisores e novos usuários do TI são cadastrados pelo próprio site, no botão de engrenagem.
