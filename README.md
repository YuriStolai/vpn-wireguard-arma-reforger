# VPN WireGuard para Arma Reforger

Este projeto pretende permitir o acesso público a um servidor de Arma Reforger
executado em uma rede local, sem configurar redirecionamento de portas no
roteador. Um VPS com endereço IP público recebe o tráfego dos jogadores e o
encaminha ao computador hospedeiro por meio de um túnel WireGuard.

O projeto está na fase de planejamento. A arquitetura detalhada, as portas e os
protocolos do jogo, as regras de encaminhamento e os ambientes suportados ainda
serão definidos antes da implementação.

## Como funciona

```text
Jogador (PC ou console)
          |
          | IP e porta públicos do VPS
          v
     VPS público
          |
          | túnel WireGuard
          v
Hospedeiro na rede local
          |
          v
Servidor de Arma Reforger
```

O hospedeiro inicia uma conexão de saída com o VPS e mantém o túnel ativo. Os
jogadores acessam somente o endpoint público do VPS e não precisam instalar o
WireGuard, importar chaves ou alterar a configuração de rede de seus
dispositivos. O tráfego de resposta retorna aos jogadores pelo mesmo VPS.

## Objetivos

- evitar alterações no roteador da rede local;
- oferecer aos jogadores um endpoint público convencional;
- permitir conexões de PC, PlayStation 5 e Xbox sem uma VPN no cliente;
- expor somente as portas necessárias ao jogo e à administração da VPN;
- manter a complexidade da rede restrita ao VPS e ao hospedeiro;
- fornecer procedimentos reproduzíveis de instalação, validação e diagnóstico.

## Escopo

O projeto incluirá a arquitetura de rede, a configuração do túnel WireGuard, o
encaminhamento do tráfego entre o VPS e o hospedeiro, controles mínimos de
segurança, exemplos sem dados sensíveis e verificações de conectividade.

Não fazem parte do escopo:

- instalar ou administrar o servidor de Arma Reforger além de sua
  conectividade;
- configurar port forwarding no roteador local;
- instalar WireGuard nos dispositivos dos jogadores;
- administrar contas, permissões ou banimentos dentro do jogo;
- garantir a disponibilidade de serviços ou infraestrutura de terceiros.

## Pré-requisitos previstos

- um VPS com endereço IP público e permissão para abrir as portas necessárias;
- um computador hospedeiro capaz de executar o WireGuard e iniciar conexões de
  saída;
- um servidor de Arma Reforger executado no hospedeiro;
- acesso administrativo ao VPS e ao hospedeiro.

As distribuições e versões suportadas serão documentadas após a definição da
arquitetura.

## Estado do projeto

Ainda não há scripts, configurações prontas ou comandos de instalação. Antes do
início da implementação, precisam ser definidos:

- o provedor, a região e o sistema operacional do VPS;
- o sistema operacional do hospedeiro;
- o plano de endereçamento da VPN;
- as portas e os protocolos usados pelo Arma Reforger;
- as regras de firewall, NAT e roteamento;
- as metas de latência, perda de pacotes e jogadores simultâneos;
- o armazenamento seguro das configurações de cada peer.

Os próximos entregáveis previstos incluem um diagrama de rede, um modelo de
ameaças, modelos de configuração com sufixo `.example`, scripts idempotentes,
um guia de operação e testes de conectividade.

## Segurança

Nunca versione chaves privadas, endereços públicos reais, endpoints ou
configurações específicas das máquinas. O VPS e o hospedeiro devem usar pares
de chaves WireGuard distintos, e as regras de rede devem seguir o princípio do
menor privilégio.

Revise cuidadosamente qualquer script antes de executá-lo com privilégios
administrativos. Testes e procedimentos devem evitar alterações permanentes no
firewall ou na rede e oferecer uma forma explícita de reversão quando fizerem
mudanças temporárias.

## Documentação

A especificação atual, incluindo requisitos, critérios de aceitação, restrições
e decisões pendentes, está em [`specs/specs.md`](specs/specs.md).
