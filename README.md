# MainAlto GitSignals — canal público criptografado

**Estado: CONTRATO V2 PUBLICADO / CHAVE PÚBLICA LOCAL AINDA NÃO REGISTRADA / NÃO ATIVADO.**

Este repositório público transporta exclusivamente **sinais criptografados e metadados mínimos**. Código privado, logs locais, prompts em texto aberto, segredos e resultados sensíveis não são publicados. O código do motor permanece em `alexbonemberger/chat-automation` (privado).

## Proteção de confidencialidade

1. A **chave privada RSA** fica exclusivamente no Windows do operador; nunca será colocada no GitHub, no chat, em Actions ou em recibos.
2. A **chave pública RSA (SPKI PEM)** pode ser publicada em `keys/active-public.pem`, após ser obtida do Windows. O arquivo ainda não existe: NÃO inventar chave nem aceitar chave de identidade desconhecida.
3. Para cada solicitação, o controlador gera 32 bytes aleatórios (AES-256-GCM), nonce aleatório de 12 bytes e tag de 16 bytes. A chave AES é cifrada com RSA-OAEP-SHA256.
4. O envelope `requests/<request_id>.json` contém somente `schema`, `request_id`, `key_id`, `algorithm`, `wrapped_key`, `nonce`, `tag` e `ciphertext`.
5. `key_id` = SHA-256 em hex minúsculo de `RSA.ExportSubjectPublicKeyInfo()` (DER), não hash dos bytes PEM.
6. AAD = UTF-8 da string exata `mainalto.gitsignals.envelope/2\n<request_id>\n<key_id>\n`. Uma alteração de ID/chave invalida a autenticação GCM.
7. Dentro do ciphertext: JSON v1 **privado** com `schema=mainalto.gitsignals.request/1`, `request_id`, `operation=capability.propose`, `chat`, `capability`, `objective`, `model=qwen3-vl:2b-instruct`, `expires_utc`. Todos os campos internos são validados **somente após** autenticação e descriptografia.
8. Cada ID é único e imutável. O receptor valida branch, hash dos bytes recebidos, TTL (máximo 7 dias), conversa exata e `STOP`, sem replay automático de efeito incerto.

## O que é público

- `contracts/request.schema.json`: somente a estrutura do **envelope cifrado** (v2).
- `examples/envelope.shape.json`: exemplo estrutural **não cifrado de verdade**, não despachável.
- `keys/active-public.pem`: chave pública, somente **depois** de fornecida pelo Windows.
- `requests/`: futuramente, exclusivamente envelopes cifrados.
- `.github/workflows/`: testes de estrutura e transporte público; não podem ler segredos locais ou buscar o código privado.
- Nenhum `receipt.json` detalhado ou `proposal.cs` local deve ser publicado aqui.

## Autoridade e limite de segurança

**Criptografia não é autorização.** Qualquer pessoa com acesso à chave pública pode criar ciphertext válido; o executor exige origem Git permitida, request_id único, escopo `capability.propose` e autorização local. Publicar um sinal NÃO permite execução C# no Windows, instalação, alteração de arquivos de produção nem ampliação de permissões.

O Qwen pode **propor** C#; testes isolados exigem uma sandbox efetiva e a delegação já definida. Efeitos locais reais ou novas permissões exigem aprovação explícita do operador vinculada ao SHA-256 da proposta e ao escopo.

## Compatibilidade e ativação

O formato antigo de solicitações v1 em texto claro está **desabilitado** para o consumidor v2. Não existe sinal operacional em `requests/`. A instalação do consumidor e o registro da chave pública dependem de operação local autorizada, que ainda não ocorreu.

### Referências

- [RSA OAEP SHA-256 no .NET](https://learn.microsoft.com/en-us/dotnet/api/system.security.cryptography.rsaencryptionpadding.oaepsha256)
- [AES-GCM no .NET](https://learn.microsoft.com/en-us/dotnet/api/system.security.cryptography.aesgcm)
- [GitHub Actions e uso público gratuito](https://docs.github.com/en/actions/concepts/billing-and-usage)

**Evidência:** `PUBLIC_CONTRACT_CI=PASS` não comprova motor privado compilado, criptografia ponta-a-ponta, UIA nem ciclo autônomo.
