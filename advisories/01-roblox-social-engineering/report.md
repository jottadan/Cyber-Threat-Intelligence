# Relatório de Cyber Threat Intelligence
## Possível phishing de credenciais no Roblox por engenharia social in-game

> **Status:** Investigação Preliminar (Testes em Laboratório)  
> **Data:** 12/11/2026  
> **Autor:** João P.  
> **Escopo:** Avaliação de solicitações de amizade e engenharia social *in-game*

---

## Resumo executivo

Relatos públicos alegaram que usuários do Roblox poderiam perder suas contas simplesmente ao aceitar solicitações de amizade de determinados perfis. A investigação buscou avaliar se essa ação, isoladamente, expõe dados capazes de permitir a tomada de controle de uma conta e examinar uma hipótese alternativa baseada em engenharia social e phishing.

Foram utilizadas duas contas controladas para observar o tráfego associado a solicitações de amizade, bloqueio/desbloqueio e interações pelo chat. De acordo com os resultados registrados, os testes não identificaram evidências de que essas interações, por si só, fornecessem à conta observadora credenciais, tokens de autenticação ou outros dados suficientes para assumir o controle da conta-alvo. O material menciona um dado associado à conta presente no corpo de uma requisição, mas não foi possível observar meios de ter acesso a essa informação sensível, sem que fosse o dono da própria conta, portanto, ele não pode ser classificado neste relatório como credencial ou token explorável.

Em paralelo, foi reproduzido um cenário controlado de engenharia social dentro de uma experiência de teste. A documentação descreve uma interface de falsa verificação/moderação, um campo de entrada e a transmissão de um valor fictício do cliente para o servidor da experiência por meio de um `RemoteEvent`. O cenário demonstra que uma interface enganosa dentro de uma experiência pode solicitar uma informação e encaminhá-la ao servidor controlado. A documentação apresentada, entretanto, não comprova por si só a coleta de credenciais reais, a transmissão para uma infraestrutura externa ou o comprometimento efetivo de uma conta Roblox.

**Julgamento principal:** os testes descritos não corroboram a alegação de que aceitar uma solicitação de amizade seja, por si só, um mecanismo direto de comprometimento. Phishing mediado por engenharia social é uma hipótese plausível para explicar alguns relatos, mas a ligação entre a prova de conceito (PoC) e incidentes reais permanece não confirmada.

**Implicação defensiva:** usuários não devem fornecer senhas ou códigos de autenticação em interfaces de experiências, mensagens ou supostos fluxos de moderação. Para confirmar a hipótese de campanha, são necessárias evidências adicionais dos relatos originais, do fluxo real de coleta e de eventuais URLs ou infraestruturas externas.

 # 1. O que está acontecendo? (O Contexto)
Circulou nas redes sociais (como o TikTok) o alerta de que aceitar solicitações de amizade de determinados perfis no Roblox seria suficiente para perder o controle da conta. 

Decidi investigar esse boicote: *será que um clique em "Aceitar amizade" realmente rouba dados, ou existe uma história de engenharia social por trás disso?*

---

## 2. Como eu testei? (Metodologia de Laboratório)
Para responder à pergunta com dados reais e não apenas achismos, criei um ambiente controlado com duas contas de teste:
* *Conta A:* Atuou como o "perfil suspeito / atacante".
* *Conta B:* Atuou como a "vítima".

*Testes executados:*
1. Envio de solicitação de amizade (monitorando o tráfego gerado via Portswigger).
2. Ciclo de bloqueio e desbloqueio entre as contas.
3. Conversas e interações via chat.
4. Criação de uma *Prova de Conceito (PoC)* de uma interface de falsa verificação/moderação dentro de uma experiência controlada, testando se dados digitados pelo usuário chegam ao servidor via RemoteEvent. [PoC disponível aqui.](evidence.md)

Tudo com o foco de descobrir se realmente existe uma falha de segurança nativa da plataforma que permite tomar as credenciais de uma conta com uma ação benigna.

---

## 3. O que eu descobri? (Achados Técnicos)
* *Sobre a amizade e o chat:* O tráfego capturado mostrou apenas metadados básicos (nome de usuário, IDs). *Nenhum token de sessão ou credencial foi exposto* apenas por aceitar a amizade ou trocar mensagens. A alegação de comprometimento automático é, até onde pude investigar, falsa no escopo testado.
* *Sobre a engenharia social (*A PoC):* Embora o clique na amizade seja inofensivo, a simulação provou que **é totalmente viável criar interfaces enganosas (*falsos avisos de moderação) dentro de um jogo** para induzir o usuário a digitar informações e enviá-las ao servidor da experiência (RemoteEvent).

> *Conclusão parcial:* Aceitar amizade não aparenta permitir acesso a dados sensíveis por si só. É comprovado que existe um vetor de risco que permite *engenharia social subsequente* (o golpista te convence a ir para um jogo falso e digitar seus dados lá), porém nos testes realizados não foi possível verificar se realmente é possível extrair credenciais reais para um servidor externo do Roblox.

---

## 4. Lições Defensivas
* *Para jogadores:* Nunca insirir senhas ou códigos de segurança (2FA) em telas que aparecem dentro de jogos, por mais oficiais que pareçam. O Roblox nunca pede senha via pop-up de experiência.
* *Para analistas:* Sempre separar o mito da internet da realidade técnica. Relatos públicos muitas vezes misturam etapas (o contato inicial vs. o golpe real).

---

## 5. O que falta investigar? (Próximos Passos)
Para fechar o caso com 100% de certeza no futuro, eu precisaria de:
* Entrar em um jogo criado para esse objetivo.
* Analisar se há exfiltração de dados para servidores externos fora do ecossistema do Roblox.

## Executive summary

Public reports claimed that Roblox users could lose their accounts simply by accepting friend requests from certain profiles. The investigation sought to assess whether this action, in isolation, exposes data capable of allowing control of an account and examine an alternative hypothesis based on social engineering and phishing.

Two controlled accounts were used to observe traffic associated with friend requests, blocking/unblocking and chat interactions. Based on recorded results, testing did not identify evidence that these interactions alone provided the observer account with sufficient credentials, authentication tokens, or other data to take control of the target account. The material mentions data associated with the account present in the body of a request, but it was not possible to observe ways of accessing this sensitive information without being the owner of the account itself, therefore, it cannot be classified in this report as a credential or exploitable token.

In parallel, a controlled social engineering scenario was reproduced within a test experiment. The documentation describes a mock check/moderation interface, an input field, and the passing of a dummy value from the client to the experiment server via a RemoteEvent. The scenario demonstrates that a deceptive interface within an experience can request information and forward it to the controlled server. The documentation presented, however, does not in itself prove the collection of real credentials, the transmission to an external infrastructure or the effective compromise of a Roblox account.

Main judgment: the tests described do not support the claim that accepting a friend request is, in itself, a direct commitment mechanism. Phishing mediated by social engineering is a plausible hypothesis to explain some reports, but the link between proof of concept (PoC) and real incidents remains unconfirmed.

Defensive implication: Users should not provide passwords or authentication codes in experience interfaces, messages, or supposed moderation flows. To confirm the campaign hypothesis, additional evidence is needed from the original reports, the actual collection flow and any external URLs or infrastructures.
