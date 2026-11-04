# Investigação — Campanha de Phishing no Roblox

Investigação sobre relatos de que aceitar determinadas solicitações de
amizade no Roblox poderia resultar diretamente no comprometimento da
conta.

## Resumo

Foram utilizadas duas contas controladas para analisar o tráfego
gerado durante:

* solicitações de amizade;
* bloqueio e desbloqueio;
* interações pelo chat.

Não foi observada nenhuma informação sensível que permitisse à Conta A
assumir o controle da Conta B.

Posteriormente, foi possível reproduzir um cenário de phishing
envolvendo uma falsa situação de moderação, uma tela de login
fraudulenta e envio das credenciais fornecidas pelo usuário para uma
infraestrutura externa.

## Cadeia de ataque

```text
Solicitação de amizade
        ↓
Vítima aceita
        ↓
Engenharia social
        ↓
Falsa situação de moderação
        ↓
Falsa tela de login
        ↓
Vítima fornece as credenciais
        ↓
Coleta externa das credenciais
```

## Conclusão

Os testes não apresentaram evidências de que o simples aceite de uma
solicitação de amizade comprometa diretamente uma conta.

O cenário reproduzido é mais compatível com uma campanha de phishing e
engenharia social, utilizando a solicitação de amizade como ponto
inicial de contato com a vítima.
