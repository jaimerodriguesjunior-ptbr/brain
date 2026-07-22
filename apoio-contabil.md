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
