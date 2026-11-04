# Relatório de Cyber Threat Intelligence
## Possível phishing de credenciais no Roblox por engenharia social in-game

| Campo | Valor |
|---|---|
| Tipo de produto | Análise de ameaça com foco tático e contextualização operacional |
| Status | Preliminar — cenário reproduzido em ambiente controlado; campanha real não confirmada |
| Data desta versão | 24/10/2026 |
| Analista | João P. |
| Classificação de compartilhamento | Público |
| Identificador do caso | João P. |

---

## 1. Resumo executivo

Relatos públicos alegaram que usuários do Roblox poderiam perder suas contas simplesmente ao aceitar solicitações de amizade de determinados perfis. A investigação buscou avaliar se essa ação, isoladamente, expõe dados capazes de permitir a tomada de controle de uma conta e examinar uma hipótese alternativa baseada em engenharia social e phishing.

Foram utilizadas duas contas controladas para observar o tráfego associado a solicitações de amizade, bloqueio/desbloqueio e interações pelo chat. De acordo com os resultados registrados, os testes não identificaram evidências de que essas interações, por si só, fornecessem à conta observadora credenciais, tokens de autenticação ou outros dados suficientes para assumir o controle da conta-alvo. O material menciona um dado associado à conta presente no corpo de uma requisição, mas não foi possível observar meios de ter acesso a essa informação sensível, sem que fosse o dono da própria conta, portanto, ele não pode ser classificado neste relatório como credencial ou token explorável.

Em paralelo, foi reproduzido um cenário controlado de engenharia social dentro de uma experiência de teste. A documentação descreve uma interface de falsa verificação/moderação, um campo de entrada e a transmissão de um valor fictício do cliente para o servidor da experiência por meio de um `RemoteEvent`. O cenário demonstra que uma interface enganosa dentro de uma experiência pode solicitar uma informação e encaminhá-la ao servidor controlado. A documentação apresentada, entretanto, não comprova por si só a coleta de credenciais reais, a transmissão para uma infraestrutura externa ou o comprometimento efetivo de uma conta Roblox.

**Julgamento principal:** os testes descritos não corroboram a alegação de que aceitar uma solicitação de amizade seja, por si só, um mecanismo direto de comprometimento. Phishing mediado por engenharia social é uma hipótese plausível para explicar alguns relatos, mas a ligação entre a prova de conceito (PoC) e incidentes reais permanece não confirmada.

**Implicação defensiva:** usuários não devem fornecer senhas ou códigos de autenticação em interfaces de experiências, mensagens ou supostos fluxos de moderação. Para confirmar a hipótese de campanha, são necessárias evidências adicionais dos relatos originais, do fluxo real de coleta e de eventuais URLs ou infraestruturas externas.

## 2. Questão de inteligência e objetivo

### Questão principal

A aceitação de uma solicitação de amizade no Roblox permite o comprometimento direto de uma conta ou pode funcionar apenas como etapa inicial de uma abordagem de engenharia social destinada a coletar credenciais?

### Objetivos específicos

1. Avaliar, dentro do escopo dos testes realizados, se as interações de amizade, bloqueio/desbloqueio e chat expõem informações que permitam assumir a conta-alvo.
2. Documentar a mecânica demonstrada pela PoC de interface enganosa e transmissão de entrada do usuário.
3. Distinguir observações reproduzidas em laboratório de alegações presentes em fontes públicas.
4. Identificar lacunas de inteligência e evidências necessárias para confirmar ou refutar a hipótese de campanha.
5. Registrar implicações defensivas para usuários e possíveis linhas de investigação para analistas.

## 3. Avaliação da ameaça — julgamentos-chave

| ID | Julgamento | Confiança | Fundamentação e limite |
|---|---|---|---|
| J-01 | Nos testes descritos, não foi observada evidência de que aceitar uma solicitação de amizade, isoladamente, comprometesse a conta-alvo. | Alta, restrita ao escopo testado | O resultado decorre das interações controladas documentadas. Não demonstra que todas as funcionalidades, versões ou vulnerabilidades possíveis tenham sido examinadas. |
| J-02 | A PoC demonstra que uma interface criada pelo investigador pode transmitir um valor fictício inserido pelo usuário ao servidor da experiência por um `RemoteEvent`. | Alta, conforme a descrição da evidência | O material descreve o caminho cliente → `FireServer()` → `RemoteEvent` → servidor. As imagens originais e o código completo devem ser preservados para revisão independente. |
| J-03 | A engenharia social in-game seguida de uma solicitação enganosa de informação é uma hipótese plausível para explicar parte dos relatos públicos. | Baixa a moderada | O mecanismo é plausível e foi parcialmente reproduzido, mas não há correlação independente apresentada entre a PoC e vítimas ou incidentes reais. |
| J-04 | A ocorrência de coleta de credenciais reais, exfiltração para infraestrutura externa e tomada de conta não foi demonstrada pelo conjunto de evidências descrito. | Alta quanto à insuficiência do material apresentado | A PoC usou um valor fictício e documenta a comunicação com o servidor de teste. Não foram apresentados, neste conjunto, registros verificáveis de credenciais reais, destino externo ou autenticação indevida. |
| J-05 | Não há base suficiente para atribuir os relatos a um ator, grupo, infraestrutura ou campanha coordenada específica. | Alta | Não foram fornecidos identificadores de ator, indicadores técnicos, correlações entre incidentes ou artefatos de infraestrutura. |

### Definição de confiança utilizada

- **Alta:** evidência direta sustenta o julgamento dentro de um escopo delimitado; a confiança não deve ser extrapolada para além desse escopo.
- **Moderada:** interpretação sustentada por evidências parciais, mas com alternativas ou lacunas relevantes.
- **Baixa:** hipótese plausível, porém baseada em evidência indireta ou insuficiente para confirmação.

A confiança diz respeito ao julgamento analítico, não à gravidade potencial do risco.

## 4. Contexto e origem dos relatos

A investigação foi motivada por publicações em redes sociais que associavam o comprometimento de contas à aceitação de solicitações de amizade de determinados usuários. O material fornecido menciona um relato no TikTok que descrevia também um convite para uma experiência/jogo. Esse detalhe levou à formulação de uma hipótese alternativa: a interação de amizade poderia estabelecer contato com a vítima, enquanto o comprometimento seria tentado posteriormente por meio de engenharia social.

O conteúdo integral, a URL, a data de publicação, o identificador do autor e uma captura preservada do relato original não foram incluídos no material recebido para a elaboração deste documento. Assim, a existência e o conteúdo dos relatos são tratados como contexto reportado, não como incidentes independentemente verificados.

A expressão **“campanha”** é usada aqui como hipótese de trabalho, não como conclusão de que exista uma operação coordenada. Para afirmar uma campanha real, seria necessário identificar recorrência, infraestrutura compartilhada, padrões comuns ou outros vínculos independentes entre ocorrências.

## 5. Escopo e metodologia

### Ambiente

- **Conta A:** conta controlada utilizada para realizar interações.
- **Conta B:** conta controlada utilizada como alvo dos testes.
- **Experiência de teste:** ambiente controlado criado para demonstrar uma interface e a comunicação entre cliente e servidor.

### Testes relatados

1. Envio de solicitação de amizade da Conta A para a Conta B e análise do tráfego gerado.
2. Bloqueio e posterior desbloqueio da Conta B pela Conta A.
3. Interações entre as contas pelo sistema de chat.
4. Criação e execução de uma interface de falsa verificação/moderação em uma experiência controlada.
5. Envio de um valor fictício da interface cliente para o servidor de teste por meio de um `RemoteEvent`.
6. Exibição de uma caixa de diálogo de expulsão como parte da simulação.

### Limites metodológicos

A análise descrita foi limitada ao comportamento observável pelo cliente, às interações testadas e ao cenário controlado criado pelo investigador. Não foi fornecida uma matriz completa de endpoints, campos de requisição, horários, capturas de tráfego, logs brutos ou versões da plataforma utilizadas. Também não foi apresentada evidência independente de uma vítima real ou de uma infraestrutura externa associada aos relatos.


## 6. Reconstrução do comportamento

É necessário distinguir dois fluxos: **(A) a hipótese de ataque relatada** e **(B) a sequência efetivamente descrita na PoC controlada**. Eles não devem ser apresentados como se fossem uma única cadeia confirmada.

### A. Cadeia de ataque hipotética

```text
Contato inicial / solicitação de amizade
                 ↓
A vítima aceita ou interage com o perfil
                 ↓
O interlocutor estabelece contato ou convida a vítima
para uma experiência
                 ↓
É apresentada uma situação enganosa de moderação/verificação
                 ↓
A vítima é induzida a fornecer informação
                 ↓
Possível coleta de credenciais e envio a um destino controlado
                 ↓
Possível uso das credenciais para tentar assumir a conta
```

**Estado analítico:** hipótese plausível, não confirmada como sequência de um incidente real. As etapas de coleta de credenciais reais, envio para destino externo e tomada de conta exigem evidência adicional.

### B. Sequência documentada na PoC controlada

```text
Usuário interage com a interface de teste
                 ↓
A interface recebe um valor fictício
                 ↓
LocalScript chama FireServer()
                 ↓
RemoteEvent (SimulationSubmit)
                 ↓
Servidor da experiência recebe/processa o valor
                 ↓
A simulação exibe uma expulsão do jogador
```

**Estado de evidência:** a documentação fornecida descreve essa sequência como reproduzida em ambiente controlado. O valor utilizado era fictício. A execução demonstra a transmissão de uma entrada ao servidor da experiência, mas não demonstra comprometimento de conta Roblox nem, por si só, a transmissão de dados para infraestrutura externa.

## 7. Resultados técnicos

### 7.1 Solicitação de amizade

A Conta A enviou uma solicitação de amizade para a Conta B. Conforme o registro original, o tráfego observado continha informações relacionadas à plataforma, como nome de usuário, identificador de usuário e identificadores de itens. Não foi identificada, na interação descrita, informação que permitisse à Conta A autenticar-se como Conta B ou tomar controle da conta.

O relatório também cita um dado considerado sensível no corpo de uma requisição, mas não descreve seu nome, conteúdo ou função. Não é possível avaliar sua sensibilidade apenas com essa descrição. Para fechar esse ponto, devem ser registrados: endpoint e método HTTP, nome do campo, origem e destino da requisição, pré-requisitos de autenticação, se o dado é visível a outra conta e se ele pode ser reutilizado para autenticação.

### 7.2 Bloqueio e desbloqueio

A Conta A bloqueou e posteriormente desbloqueou a Conta B. O investigador não observou dados capazes de autenticar como a conta-alvo ou permitir sua tomada de controle durante essas interações. Esse resultado vale para o fluxo e as condições examinados.

### 7.3 Chat

Foram realizadas interações pelo sistema de chat entre as contas controladas. O tráfego foi observado em busca de credenciais, tokens de autenticação ou outros dados que pudessem permitir o comprometimento da conta-alvo. De acordo com o material, nenhum dado desse tipo foi identificado.

### 7.4 Interface de falsa verificação/moderação

A documentação descreve uma interface dentro da experiência de teste, construída com elementos do `StarterGui` e um `ScreenGui`. A interface simula uma situação enganosa de verificação/moderação e apresenta um campo de entrada e um botão de continuação.

A descrição da evidência caracteriza a interface como uma simulação de verificação e esclarece que ela não tinha como objetivo reproduzir uma página legítima de login. Isso é relevante: embora o relatório original descreva a hipótese em termos de phishing de credenciais, a documentação visual apresentada não basta, isoladamente, para afirmar que a PoC reproduziu uma tela de login do Roblox ou coletou nome de usuário e senha.

### 7.5 Comunicação entre cliente e servidor

O objeto `SimulationSubmit`, descrito como `RemoteEvent` em `ReplicatedStorage`, foi utilizado para representar a comunicação entre o cliente e o servidor. Após a interação com o botão, um `LocalScript` envia o valor fictício por `FireServer()`, e o servidor da experiência recebe/processa essa entrada.

O achado demonstra a mecânica de transmissão de um dado fornecido na interface para o servidor da experiência. Não demonstra que esse comportamento exista em experiências de terceiros, que o dado seja uma credencial, que tenha sido persistido ou encaminhado para um sistema externo, nem que possa ser usado para autenticação na conta Roblox.

### 7.6 Expulsão da experiência

A caixa de diálogo de expulsão foi implementada como parte da simulação, para representar um evento de moderação falso e reforçar o contexto enganoso. A expulsão é descrita como uma funcionalidade legítima da plataforma utilizada exclusivamente no ambiente de teste. Sua presença na PoC demonstra o recurso visual/temporal da encenação, não uma ação de moderação real nem um mecanismo de comprometimento.

## 8. Mapeamento comportamental — MITRE ATT&CK

O MITRE ATT&CK pode ajudar a padronizar a descrição de comportamentos, mas seu escopo Enterprise é orientado a intrusões em ambientes corporativos. O caso em análise é um possível golpe contra contas de consumidores em uma plataforma de jogos. Portanto, qualquer mapeamento abaixo é **provisório e descritivo**, não uma atribuição de ator ou confirmação de técnica em uma campanha real.

| Técnica | Aplicabilidade ao cenário | Condição / ressalva |
|---|---|---|
| [T1566.003 — Phishing: Spearphishing via Service](https://attack.mitre.org/techniques/T1566/003/) | Candidato condicional para a etapa em que um serviço de terceiros é utilizado para estabelecer contato e conduzir a vítima a uma ação enganosa. | Usar apenas se houver evidência de mensagens ou contato digital via Roblox empregados para entregar a abordagem de phishing. A técnica pertence ao domínio Enterprise e pode não representar perfeitamente um golpe de consumo. |
| [T1056.003 — Input Capture: Web Portal Capture](https://attack.mitre.org/techniques/T1056/003/) | **Não atribuída nesta versão.** | A evidência descrita mostra uma interface dentro de uma experiência e um valor fictício enviado ao servidor. Não demonstra captura em um portal web falso nem coleta de credenciais reais conforme a definição da técnica. |

O mapeamento deve ser revisto quando houver artefatos adicionais do incidente — por exemplo, mensagens preservadas, URLs, captura de uma página de autenticação falsa ou evidência de uso das credenciais. Não se deve selecionar uma técnica apenas porque seu nome parece semelhante ao comportamento observado.

## 9. Implicações defensivas

### Para usuários

- Não inserir senhas, códigos de autenticação ou códigos de recuperação em interfaces de jogos, formulários de experiências ou supostas telas de verificação/moderação.
- Tratar mensagens urgentes que aleguem punição, expulsão, verificação obrigatória ou perda iminente de conta como possíveis mecanismos de pressão social.
- Acessar a autenticação por canais oficiais, digitando o endereço oficial ou utilizando o aplicativo legítimo, em vez de confiar em interfaces ou links encaminhados por desconhecidos.
- Em caso de suspeita, alterar a senha pelos canais oficiais, revisar sessões e métodos de recuperação disponíveis e reportar o perfil ou a experiência à plataforma.

### Para equipes de segurança, moderação ou Trust & Safety

- Preservar as URLs, nomes de usuário, IDs de experiência, IDs de usuário, mensagens e horários observados em ocorrências reais, evitando depender apenas de capturas isoladas ou relatos recontados.
- Investigar se experiências ou perfis diferentes reutilizam os mesmos textos, fluxos de interface, domínios ou destinos de coleta.
- Quando houver acesso legítimo a telemetria da plataforma, correlacionar denúncias, convites para experiências, eventos de moderação simulados e padrões de interação entre contas. A disponibilidade desses dados depende dos recursos e permissões da plataforma.
- Tratar a presença de uma interface enganosa como sinal para investigação, não como prova isolada de comprometimento.
- Evitar publicar ou bloquear indicadores sem contexto. Nenhum domínio, URL, endereço IP, hash ou identificador de infraestrutura foi fornecido neste material para inclusão como IOC.

## 10. Lacunas de inteligência e próximos passos

As seguintes questões devem orientar a continuação da investigação, priorizadas pela capacidade de confirmar ou refutar a hipótese.

| Prioridade | Lacuna | Evidência ou ação necessária | Valor para a análise |
|---|---|---|---|
| Alta | O que os relatos públicos originais afirmam exatamente? | Registrar URLs, datas, autor/perfil, capturas integrais e contexto das publicações citadas. | Permite avaliar a fonte e comparar os relatos sem depender de paráfrases. |
| Alta | A PoC reproduziu coleta de credenciais ou apenas o envio de um valor genérico? | Documentar a interface completa, o campo solicitado, o código relevante e o tipo de valor utilizado; manter a prova baseada exclusivamente em dados fictícios. | Define com precisão o comportamento efetivamente demonstrado. |
| Alta | Houve comunicação com infraestrutura externa? | Anexar registros de rede da PoC, destinos, protocolos e horários, distinguindo comunicação local/da experiência de conexões externas. | Necessário para sustentar a alegação de exfiltração externa. |
| Alta | Existe evidência de uma conta real comprometida por esse fluxo? | Obter relatos primários verificáveis ou documentação técnica consentida e sanitizada; distinguir tentativa, fornecimento de dados e takeover confirmado. | Liga a hipótese técnica ao impacto real. |
| Média | Os relatos descrevem uma sequência comum ou golpes diferentes? | Comparar texto, interface, sequência de eventos, IDs e domínios entre relatos independentes. | Avalia se existe um padrão comum ou uma campanha coordenada. |
| Média | É possível identificar indicadores recorrentes? | Coletar e validar URLs, domínios, IDs de experiências/contas e hashes de artefatos, se disponíveis. | Apoia correlação e comunicação defensiva. |
| Média | Qual é a natureza do campo considerado sensível na requisição? | Registrar nome do campo, sem expor segredos, e demonstrar sua função e requisitos de acesso. | Evita tanto minimizar uma exposição real quanto classificá-la incorretamente. |

## 11. Limitações

1. Os testes foram feitos com contas controladas e cobriram apenas as interações descritas.
2. Não foram fornecidos logs brutos completos, PCAP/HAR, endpoints, horários, versões ou hashes dos artefatos de evidência para revisão independente nesta etapa.
3. Os relatos públicos que motivaram a investigação não vieram acompanhados de URLs e metadados suficientes para verificar sua origem e conteúdo.
4. A PoC utilizou valores fictícios e não envolveu credenciais reais; portanto, não demonstra comprometimento real de uma conta.
5. A evidência descrita confirma comunicação entre cliente e servidor da experiência de teste, mas não basta para confirmar armazenamento ou encaminhamento para infraestrutura externa.
6. O relatório não atribui o comportamento a indivíduo, grupo ou operação coordenada.
7. Os resultados não descartam vulnerabilidades, técnicas ou caminhos de comprometimento diferentes daqueles testados.

## 12. Conclusão

Nos testes descritos, não foram encontradas evidências de que aceitar uma solicitação de amizade, por si só, comprometesse diretamente a conta-alvo. As interações de amizade, bloqueio/desbloqueio e chat examinadas não revelaram dados que demonstrassem capacidade de autenticação como a conta-alvo.

A PoC controlada demonstra que uma interface enganosa dentro de uma experiência pode receber uma entrada fictícia e enviá-la ao servidor da própria experiência por meio de um `RemoteEvent`. Esse resultado sustenta a plausibilidade de uma mecânica de engenharia social in-game, mas não comprova a coleta de credenciais reais, exfiltração para infraestrutura externa ou comprometimento de conta. A hipótese de phishing é compatível com os relatos descritos, mas sua aplicação a incidentes reais permanece não confirmada.

Assim, a formulação mais bem sustentada pelas evidências disponíveis é: **a alegação de comprometimento automático decorrente da aceitação de uma solicitação de amizade não foi corroborada pelos testes realizados; engenharia social e phishing constituem uma hipótese alternativa plausível que exige correlação com evidências de incidentes reais.**

## Apêndice A — Registro das evidências descritas

| ID | Arquivo referenciado | Conteúdo descrito | O que pode sustentar | Limite da evidência descrita |
|---|---|---|---|---|
| EV-001 | `1.` | Estrutura de `ReplicatedStorage` com `RemoteEvent` chamado `SimulationSubmit`. | Existência do objeto de comunicação no ambiente de teste. | Não comprova, isoladamente, que dados foram enviados ou para onde. |
| EV-002 | `2.` | Estrutura do `StarterGui`, incluindo `ScreenGui`. | Estrutura visual da interface no ambiente de teste. | Não estabelece que a interface exista em experiências de terceiros. |
| EV-003 | `3.` | Interface de falsa verificação, com campo de entrada e botão de continuação. | Aparência e componentes da simulação. | A descrição não demonstra uma página de login real nem coleta de credenciais reais. |
| EV-004 | `4.` | Código/execução do envio por `FireServer()` e recebimento pelo servidor, usando valor fictício. | Fluxo cliente → `RemoteEvent` → servidor de teste. | Não comprova exfiltração externa nem uso de credenciais para takeover. |
| EV-005 | `5.` | Caixa de diálogo de expulsão após a etapa de teste. | Uso de expulsão como elemento da simulação. | Não comprova moderação real, exploração ou comprometimento de conta. |

## Apêndice B — Fontes e materiais consultados

### Material primário fornecido pelo investigador

- `README` da investigação — descrição dos testes, hipótese e conclusão preliminar.
- `evidence.md` — descrições das evidências visuais da PoC controlada.
- Relato do TikTok mencionado pelo investigador — **URL, data e captura pendentes de inclusão**.
- Registros brutos de tráfego e código da PoC — **não incluídos no material para elaboração desta versão**.

### Referências metodológicas externas

- MITRE ATT&CK, **Phishing: Spearphishing via Service (T1566.003)**: https://attack.mitre.org/techniques/T1566/003/
- MITRE ATT&CK, **Input Capture: Web Portal Capture (T1056.003)**: https://attack.mitre.org/techniques/T1056/003/

---

## Executive summary

Public reports claimed that Roblox users could lose their accounts simply by accepting friend requests from certain profiles. The investigation sought to assess whether this action, in isolation, exposes data capable of allowing control of an account and examine an alternative hypothesis based on social engineering and phishing.

Two controlled accounts were used to observe traffic associated with friend requests, blocking/unblocking and chat interactions. Based on recorded results, testing did not identify evidence that these interactions alone provided the observer account with sufficient credentials, authentication tokens, or other data to take control of the target account. The material mentions data associated with the account present in the body of a request, but it was not possible to observe ways of accessing this sensitive information without being the owner of the account itself, therefore, it cannot be classified in this report as a credential or exploitable token.

In parallel, a controlled social engineering scenario was reproduced within a test experiment. The documentation describes a mock check/moderation interface, an input field, and the passing of a dummy value from the client to the experiment server via a RemoteEvent. The scenario demonstrates that a deceptive interface within an experience can request information and forward it to the controlled server. The documentation presented, however, does not in itself prove the collection of real credentials, the transmission to an external infrastructure or the effective compromise of a Roblox account.

Main judgment: the tests described do not support the claim that accepting a friend request is, in itself, a direct commitment mechanism. Phishing mediated by social engineering is a plausible hypothesis to explain some reports, but the link between proof of concept (PoC) and real incidents remains unconfirmed.

Defensive implication: Users should not provide passwords or authentication codes in experience interfaces, messages, or supposed moderation flows. To confirm the campaign hypothesis, additional evidence is needed from the original reports, the actual collection flow and any external URLs or infrastructures.
