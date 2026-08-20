# Especificação: acesso ao Arma Reforger por WireGuard

## 1. Visão geral

Este projeto deverá fornecer acesso a um servidor de Arma Reforger executado
localmente no computador do administrador. Um VPS com endereço IP público
receberá as conexões dos jogadores e encaminhará o tráfego ao computador
hospedeiro por meio de um túnel WireGuard. Não deverá ser necessário configurar
redirecionamento de portas no roteador da rede local nem instalar ou configurar
uma VPN nos dispositivos dos jogadores.

Esta especificação descreve o resultado esperado e a motivação do projeto. Os
detalhes de arquitetura e implementação deverão ser documentados em
especificações complementares antes do início da implementação.

## 2. Problema

O servidor de Arma Reforger é executado em uma rede local e, por padrão, não é
diretamente acessível por jogadores externos. A exposição convencional exigiria
alterações no roteador, como port forwarding, o que pode ser indisponível,
indesejado ou difícil de administrar.

O projeto precisa criar um caminho privado entre o VPS e o computador que
hospeda o jogo. Os jogadores deverão usar somente o endereço IP público e a
porta do VPS, como fariam ao acessar um servidor convencional. Essa abordagem é
necessária para suportar também consoles, como PlayStation 5 e Xbox, nos quais
não se deve exigir a configuração de um cliente WireGuard.

## 3. Objetivo

Permitir que jogadores em computadores ou consoles se conectem ao servidor
local de Arma Reforger informando apenas o endereço IP e a porta pública do VPS.
O tráfego deverá ser encaminhado pelo VPS ao computador hospedeiro através de
uma rede WireGuard, sem configurar port forwarding no roteador local.

## 4. Motivação

A VPN será usada para:

- eliminar a dependência de alterações no roteador local;
- ocultar dos jogadores a complexidade da VPN e da rede local;
- permitir o acesso de dispositivos sem suporte configurável ao WireGuard,
  incluindo PlayStation 5 e Xbox;
- reduzir a exposição direta do computador hospedeiro e do serviço do jogo à
  Internet;
- centralizar no VPS o endereço e as portas publicamente acessíveis;
- oferecer um processo de conexão simples e compatível com o cliente normal do
  jogo.

## 5. Escopo

### 5.1 Incluído

- definir a arquitetura de rede entre o VPS e o hospedeiro;
- configurar um túnel WireGuard entre o VPS e o hospedeiro;
- receber no IP e nas portas públicas do VPS o tráfego necessário ao Arma
  Reforger;
- encaminhar esse tráfego através da VPN até as portas correspondentes no
  computador hospedeiro;
- encaminhar corretamente ao jogador o tráfego de resposta do servidor;
- documentar instalação, configuração, conexão, validação e solução de problemas;
- fornecer exemplos de configuração sem dados sensíveis;
- definir controles mínimos de acesso à rede privada e proteção das chaves;
- criar verificações que comprovem a conectividade sem alterar permanentemente
  a rede ou o firewall do equipamento em teste.

### 5.2 Fora do escopo

- instalar, configurar ou administrar o servidor de Arma Reforger além do que
  for necessário para sua conectividade pela VPN;
- modificar o roteador local ou habilitar port forwarding;
- instalar ou configurar WireGuard nos computadores ou consoles dos jogadores;
- distribuir chaves privadas ou outros segredos pelo repositório;
- administrar contas, permissões ou banimentos dentro do Arma Reforger;
- garantir disponibilidade de serviços ou infraestrutura de terceiros.

## 6. Atores

- **Administrador:** mantém o VPS, a VPN, as regras de encaminhamento e o
  servidor local.
- **Jogador:** informa o endereço IP e a porta do VPS no cliente do Arma
  Reforger, sem participar da VPN.
- **Hospedeiro do jogo:** computador local no qual o servidor de Arma Reforger e
  o peer WireGuard são executados.
- **VPS:** servidor com endereço IP público que atua como peer WireGuard, recebe
  as conexões dos jogadores e encaminha o tráfego do jogo ao hospedeiro.

## 7. Requisitos funcionais

### RF-01 — Conexão do hospedeiro

O hospedeiro do jogo deverá estabelecer e manter um túnel WireGuard com o VPS
por meio de uma conexão de saída, sem depender de port forwarding no roteador
local.

### RF-02 — Endpoint público

O VPS deverá disponibilizar um endereço IP público e as portas necessárias para
que os jogadores localizem e acessem o servidor.

### RF-03 — Acesso ao servidor

Com o túnel ativo, o VPS deverá encaminhar o tráfego recebido nas portas públicas
do jogo para o endereço e as portas correspondentes do hospedeiro na VPN. O
tráfego de resposta deverá retornar ao jogador pelo VPS.

### RF-04 — Restrição de tráfego

A configuração deverá expor no VPS somente o tráfego necessário ao acesso ao
servidor do jogo e à administração da VPN. Outros serviços do VPS e do
hospedeiro não deverão ser disponibilizados aos jogadores por padrão.

### RF-05 — Clientes sem configuração de VPN

O jogador deverá conseguir acessar o servidor informando somente o IP e a porta
do VPS no fluxo suportado pelo jogo. A solução não deverá exigir software,
chaves, perfis de rede ou configuração WireGuard no dispositivo do jogador.

### RF-06 — Diagnóstico

A documentação deverá fornecer verificações para distinguir, no mínimo, falhas
de estabelecimento do túnel, encaminhamento, tradução de endereços, roteamento,
firewall e acesso às portas do jogo.

## 8. Requisitos não funcionais

### RNF-01 — Segurança

- O VPS e o hospedeiro deverão possuir pares de chaves WireGuard distintos.
- Chaves privadas, endereços públicos reais e configurações específicas de
  máquinas não deverão ser versionados.
- Os exemplos deverão usar valores fictícios claramente identificados.
- As regras de rede deverão seguir o princípio do menor privilégio.

### RNF-02 — Reprodutibilidade

Os procedimentos deverão ser executáveis a partir da raiz do repositório,
documentar pré-requisitos e falhar com mensagens claras quando uma dependência
estiver ausente.

### RNF-03 — Manutenibilidade

Scripts futuros deverão ser idempotentes quando viável, usar Bash com tratamento
estrito de erros e manter configurações reutilizáveis separadas de segredos.

### RNF-04 — Impacto operacional

Testes e procedimentos de configuração deverão evitar mudanças permanentes e
não intencionais no firewall ou na rede. Alterações temporárias deverão possuir
um processo explícito de limpeza ou reversão.

### RNF-05 — Desempenho

A solução deverá suportar uma sessão jogável. Metas mensuráveis de latência,
perda de pacotes e quantidade simultânea de jogadores deverão ser definidas
depois que a arquitetura e o ambiente de hospedagem forem conhecidos.

## 9. Fluxo principal

1. O administrador prepara o VPS, a infraestrutura WireGuard e as regras de
   encaminhamento.
2. O administrador cadastra o VPS e o hospedeiro como peers WireGuard.
3. O hospedeiro estabelece e mantém o túnel sem exigir uma conexão iniciada pela
   Internet contra o roteador local.
4. O jogador informa no jogo o endereço IP e a porta pública do VPS.
5. O VPS recebe os pacotes e os encaminha pelo túnel ao hospedeiro.
6. O servidor de Arma Reforger responde, e o tráfego de retorno passa pelo VPS
   até o jogador.

## 10. Critérios de aceitação

### CA-01 — Ausência de port forwarding

**Dado** que não existe regra de port forwarding no roteador do hospedeiro,
**quando** o hospedeiro ativar sua configuração WireGuard,
**então** o túnel com o VPS deverá ser estabelecido por uma conexão de saída.

### CA-02 — Acesso público ao jogo

**Dado** que o túnel entre o VPS e o hospedeiro está ativo,
**quando** um jogador informar o IP e a porta pública do VPS no jogo,
**então** o tráfego deverá alcançar o servidor de Arma Reforger e as respostas
deverão retornar ao jogador através do VPS.

### CA-03 — Isolamento de serviços

**Dado** um jogador acessando o IP público do VPS,
**quando** ele tentar acessar uma porta ou um serviço que não esteja
explicitamente autorizado,
**então** o acesso deverá ser bloqueado.

### CA-04 — Compatibilidade sem WireGuard no cliente

**Dado** um jogador em PC, PlayStation 5 ou Xbox com um cliente compatível do
Arma Reforger,
**quando** ele acessar o endpoint público do VPS,
**então** não deverá ser necessário instalar WireGuard, importar chaves ou
alterar a configuração de rede do dispositivo.

### CA-05 — Proteção de segredos

**Dado** o conteúdo versionado do repositório,
**quando** ele for inspecionado,
**então** não deverá conter chaves privadas, endpoints reais ou configurações
específicas das máquinas dos participantes.

### CA-06 — Procedimento reproduzível

**Dado** um ambiente que atenda aos pré-requisitos documentados,
**quando** o administrador seguir os procedimentos desde o início,
**então** deverá conseguir configurar e validar o caminho completo, enquanto o
jogador precisará somente do IP e da porta pública do VPS.

## 11. Restrições e premissas

- O VPS deverá possuir endereço IP público alcançável e permitir a abertura das
  portas necessárias ao Arma Reforger e ao WireGuard.
- O computador hospedeiro deverá poder iniciar conexões de saída e executar o
  WireGuard.
- Somente o VPS e o hospedeiro participarão da VPN. Os jogadores serão clientes
  externos comuns e não possuirão chaves WireGuard.
- O encaminhamento deverá preservar um caminho de retorno válido. O uso de NAT,
  roteamento ou uma combinação de ambos será definido na especificação de rede.
- Portas, protocolos e regras exatas do Arma Reforger deverão ser validados na
  especificação de rede antes da implementação.

## 12. Decisões pendentes

- escolher o provedor, a região e o sistema operacional do VPS;
- definir o sistema operacional suportado para o hospedeiro;
- definir o plano de endereçamento da VPN;
- validar portas e protocolos utilizados pelo Arma Reforger;
- decidir as regras de encaminhamento, NAT e roteamento no VPS e no hospedeiro;
- estabelecer metas de latência, perda de pacotes e número de jogadores;
- definir o método seguro de armazenamento das configurações do VPS e do
  hospedeiro.

## 13. Entregáveis previstos

- especificação de arquitetura e diagrama de rede;
- modelo de ameaças e regras de firewall;
- scripts de instalação e configuração, quando aprovados;
- modelos de configuração com sufixo `.example`;
- guia de operação para configurar, validar e substituir os peers do VPS e do
  hospedeiro;
- testes de conectividade e validação de configuração;
- atualização do `README.md` com os comandos e o ponto de entrada do projeto.

## 14. Rastreabilidade

| Objetivo | Requisitos relacionados | Critérios de aceitação |
| --- | --- | --- |
| Evitar port forwarding | RF-01 | CA-01 |
| Permitir acesso ao jogo | RF-02, RF-03 | CA-02, CA-06 |
| Não configurar os clientes | RF-05 | CA-04 |
| Limitar a exposição | RF-04, RNF-01 | CA-03, CA-05 |
| Facilitar operação e suporte | RF-06, RNF-02, RNF-04 | CA-06 |
