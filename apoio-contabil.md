# Memória do projeto Apoio Contábil

## 2026-07-21

### O que foi feito

- Investigado o fluxo de emissão de NFS-e Toledo/Equiplano pela nuvem fiscal local.
- Corrigido o suporte a certificados A1 PFX que o runtime não conseguia abrir diretamente: o caminho normal do PFX foi preservado e o fallback usa PEM com a cadeia completa somente para o erro `Unsupported PKCS12 PFX data`.
- A correção foi publicada e implantada na nuvem local na versão `c1821eb`; typecheck, build e testes passaram.
- Confirmado que a falha posterior da C S Pick não era mais de certificado: o XML era gerado, assinado e transmitido ao município.
- Comparadas as configurações da empresa autorizada `10.894.359/0001-03` e da C S Pick. O serviço `14.01.01.000` da primeira não pertence à inscrição municipal da C S Pick.
- Identificado no cadastro da Prefeitura de Toledo o serviço correto da C S Pick: `16.02.01.000 — Outros serviços de transporte de natureza municipal`.
- Ajustado o cadastro da C S Pick e confirmada a autorização da nota com o código `16.02.01.000`.
- Para testar o cadastro antigo `16.01`, foi criado e implantado suporte parametrizado por empresa para enviar item/subitem no XML. Esse modo ficou restrito à C S Pick e não altera as demais empresas.

### Problemas encontrados ou pendências

- As tentativas rejeitadas consumiram RPS e devem permanecer registradas como rejeitadas; não houve autorização para os códigos `16.01.01.000` ou `14.01.01.000` na C S Pick.
- O modo especial item/subitem da C S Pick foi necessário durante a investigação, mas o código municipal atual autorizado é `16.02.01.000`. Confirmar em uma próxima manutenção se esse modo especial deve ser removido ou mantido para compatibilidade histórica.
- O repositório local do Apoio Contábil não contém a base ativa usada na emissão; as configurações fiscais foram verificadas na nuvem local fiscal.

### Próximos passos

1. Baixo consumo de IA: verificar no banco da nuvem local se a última emissão da C S Pick permanece `autorizado` com `16.02.01.000`.
2. Baixo consumo de IA: avaliar e, se não houver mais necessidade histórica, remover a configuração especial `nfseServiceIdentityMode: item-subitem` somente da C S Pick.
3. Médio consumo de IA: atualizar a documentação operacional do cadastro de serviços municipais de Toledo, explicando que o código deve ser consultado por inscrição municipal.

### Ideias futuras

- Exibir no Apoio Contábil o código municipal efetivamente configurado e o formato de envio (`nrServico` ou item/subitem) antes da emissão.
- Registrar no histórico da empresa a resposta municipal e o código de serviço usado, para facilitar diagnóstico sem depender de consultas manuais.
- Adicionar uma validação preventiva que destaque quando o código informado não está confirmado no cadastro municipal do prestador.

## 2026-07-22

### O que foi feito

- Preparada a C S Pick para emissão NFS-e em produção após a autorização em homologação. Foi confirmada a liberação municipal de 1.000 RPS da série 1 e que nenhum RPS havia sido utilizado no ambiente de produção.
- Diagnosticado erro interno recorrente `Falha ao salvar documentos fiscais: TypeError: fetch failed` antes da transmissão ao Equiplano.
- Confirmado que a persistência reenviava todo o histórico fiscal a cada alteração: 470 documentos, totalizando aproximadamente 5,57 MB em uma única requisição ao Supabase.
- Corrigida a persistência para gravar documentos em lotes de 25 registros e repetir somente falhas transitórias de rede.
- Adicionado teste automatizado para garantir o particionamento dos registros. A suíte completa passou com 114 testes, além de typecheck e build.
- Correção publicada e implantada na nuvem fiscal local na versão `c3cabdd`.
- Validada a persistência real dos 470 documentos existentes sem criar ou transmitir nota fiscal. Os endpoints interno e público da VPS permaneceram com estado `ready`.
- Identificado que o loteamento ainda regravava todo o histórico. A persistência foi substituída por gravação incremental: cada mutação envia ao Supabase somente o documento, evento, configuração, certificado ou inutilização alterado.
- Adicionado teste que comprova que uma nova emissão persiste somente um documento. A suíte completa passou com 115 testes, além de typecheck e build.
- Persistência incremental publicada e implantada na versão `c86aeb8`; os endpoints interno e público permaneceram com estado `ready`.
- Na validação de produção, o Equiplano retornou `8011` para o RPS 380. Foi implementada recuperação automática por `esConsultarNfsePorRps` para esse código na versão `77f7258`.
- A consulta revelou que a configuração marcada como produção ainda apontava para `homologacaows`. O WSDL confirmou o endpoint de produção em `https://www.esnfs.com.br:8444/enfsws/services/Enfs.EnfsHttpsSoap11Endpoint/`.
- A seleção de endpoint passou a ser obrigatoriamente separada por ambiente na versão `3bb94b1`. A produção da C S Pick foi corrigida e a sequência restaurada para RPS 380 e lote 1, pois o portal municipal indicava último RPS utilizado igual a zero.
- Confirmado que o endpoint Equiplano de produção entrega apenas o certificado final `esnfs.com.br`, sem a intermediária `Sectigo Public Server Authentication CA DV R36`, causando `UNABLE_TO_VERIFY_LEAF_SIGNATURE` no Node.
- A intermediária indicada pelo AIA do próprio certificado foi incorporada exclusivamente ao host `www.esnfs.com.br`, porta `8444` e caminho oficial `/enfsws/services/`, preservando validação TLS e hostname. A conexão mTLS real foi validada como `authorized: true`, sem envio SOAP/RPS.
- Correção publicada e implantada na nuvem fiscal local na versão `3506225`; 118 testes, typecheck e build passaram, e o serviço permaneceu `ready` com persistência Supabase.

### Problemas encontrados ou pendências

- Ainda falta executar uma nova emissão controlada em homologação após o deploy para validar o fluxo completo pela aplicação cliente.
- Após a homologação, validar a primeira emissão em produção usando a numeração liberada pela Prefeitura.
- Falta repetir a primeira emissão em produção após a correção do endpoint e confirmar no portal municipal o consumo do RPS 380.

### Próximos passos

1. Baixo consumo de IA: emitir uma NFS-e de teste em homologação e confirmar autorização e persistência.
2. Baixo consumo de IA: confirmar no portal municipal que a homologação não afetou a sequência de produção.
3. Médio consumo de IA: emitir a primeira NFS-e em produção somente após a homologação pós-correção.

### Ideias futuras

- Adicionar métricas de tamanho e duração das gravações no Supabase para detectar crescimento antes de atingir limites de conexão.
