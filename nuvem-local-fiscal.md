# Memória do projeto Nuvem Local Fiscal

## 2026-07-21

### O que foi feito

- Investigado o fluxo NFS-e Toledo/Equiplano e corrigida a abertura de certificados A1 que eram válidos, mas não eram aceitos diretamente pelo OpenSSL do runtime.
- O caminho histórico do PFX foi preservado. O fallback converte para chave e cadeia PEM somente quando o erro é `Unsupported PKCS12 PFX data`.
- A correção foi publicada e implantada na versão `c1821eb`, com testes, typecheck e build aprovados.
- Foi implementado suporte parametrizado por empresa para a identificação Equiplano por item/subitem do serviço, sem alterar o formato das demais empresas.
- Confirmado em homologação que o serviço municipal autorizado deve corresponder ao cadastro específico do prestador na Prefeitura de Toledo.

### Problemas encontrados ou pendências

- Códigos de serviço copiados entre empresas podem ser rejeitados mesmo quando pertencem ao mesmo grupo da LC 116; o cadastro municipal do prestador é a fonte operacional correta.
- O modo item/subitem deve permanecer parametrizado por empresa e não pode se tornar comportamento global.

### Próximos passos

1. Baixo consumo de IA: manter testes de regressão para PFX normal e para o fallback PEM.
2. Médio consumo de IA: documentar no painel administrativo como confirmar o código municipal de cada prestador.

### Ideias futuras

- Exibir uma prévia segura da identidade do serviço que será enviada no XML antes da transmissão.
- Registrar no histórico técnico o formato usado: código completo ou item/subitem.

## 2026-07-22

### O que foi feito

- Diagnosticada a persistência que reenviava aproximadamente 470 documentos a cada alteração, gerando uma requisição de cerca de 5,57 MB ao Supabase.
- A persistência passou primeiro a dividir documentos em lotes e, em seguida, foi convertida para gravação incremental: somente o documento, evento, configuração, certificado ou inutilização alterado é persistido.
- As correções de persistência foram publicadas e implantadas nas versões `c3cabdd` e `c86aeb8`. A suíte passou de 114 para 115 testes e os endpoints permaneceram saudáveis.
- Implementada recuperação automática por consulta de RPS quando Toledo retorna o código `8011`, na versão `77f7258`.
- Confirmado pelo WSDL que produção e homologação da Equiplano usam endpoints distintos. A resolução por ambiente foi corrigida na versão `3bb94b1`, impedindo que uma configuração de homologação copiada seja usada silenciosamente em produção.
- Diagnosticado o erro TLS de produção: o servidor da Equiplano apresenta somente o certificado final e omite a intermediária `Sectigo Public Server Authentication CA DV R36`.
- A intermediária indicada pelo AIA do próprio certificado foi incorporada somente para o host `www.esnfs.com.br`, porta `8444` e caminho oficial `/enfsws/services/`. A validação da cadeia e do hostname permanece habilitada; não foi usado bypass TLS em produção.
- A conexão mTLS real foi validada sem envio SOAP ou RPS, retornando `authorized: true` até a raiz Sectigo R46.
- A correção TLS foi publicada e implantada na versão `3506225`. Foram aprovados 118 testes, typecheck e build; o serviço interno e o endpoint público permaneceram `ready` com persistência Supabase.
- Após o deploy, uma emissão controlada em produção foi autorizada por Toledo, com XML assinado, assinatura válida, XML e PDF disponíveis e protocolo municipal retornado.

### Problemas encontrados ou pendências

- Registros antigos de aplicações clientes podem estar marcados localmente como produção embora tenham sido transmitidos ao endpoint histórico de homologação. Eles precisam ser conferidos no portal municipal antes de serem considerados documentos com efeito fiscal.
- O pacote possui avisos de dependências reportados pelo `npm audit`; não foram alterados nesta correção para evitar ampliar o escopo fiscal.
- A primeira chamada de saúde imediatamente após o restart ocorreu antes de o processo abrir a porta; a repetição após três segundos retornou `ready`.

### Próximos passos

1. Baixo consumo de IA: confirmar no portal municipal a nota autorizada após a correção TLS e o consumo do RPS correspondente.
2. Médio consumo de IA: auditar documentos Toledo registrados como produção antes da separação de endpoints e classificá-los conforme o portal municipal.
3. Médio consumo de IA: adicionar um teste de integração periódico que valide apenas o handshake mTLS de produção, sem transmitir documento.
4. Médio consumo de IA: revisar as dependências apontadas pelo `npm audit` em uma manutenção separada, com testes fiscais completos.

### Ideias futuras

- Registrar métricas de tamanho e duração de cada persistência incremental.
- Exibir no painel técnico o endpoint efetivo resolvido por ambiente sem revelar credenciais.
- Alertar quando uma aplicação cliente rotular como produção uma resposta originada de configuração incompatível com o ambiente.
