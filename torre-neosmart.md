# Diário - Neosmart

## 22/07/2026 — tela do cliente e experiências em retrato

### O que foi feito

- Corrigida a continuidade de atendimento em duas etapas: selecionar atendimento aberto e depois experiência.
- Implementada tela do cliente persistente no Electron, com `public/abertura.mp4` em loop como estado padrão.
- Experiências ativas carregam conteúdo na mesma janela; ao sair, a tela retorna à abertura.
- Implementado repouso preto após 30 minutos de inatividade, com despertar por interação, desbloqueio, retorno da suspensão ou reconexão do monitor.
- Corrigida a exceção do Electron causada pelo uso incorreto de `getSystemIdleTime`; a chamada correta é `powerMonitor.getSystemIdleTime()`.
- Atualizados os rótulos dos controles para apresentar conteúdo ao cliente e retornar à abertura.
- Ajustado o Visagismo em retrato para vídeo preenchido e recortado corretamente, mantendo a armação virtual alinhada ao rosto.
- Movido o carrossel de armações da pesquisa para a região superior.
- Reorganizada a tela de espessura: as vistas física e de perfil calculado ficam em destaque, e a vista frontal é explicativa abaixo.
- Alinhada a escala das representações da espessura por pixels por milímetro e ampliado o canvas frontal para `520 × 520`, evitando cortes durante a rotação.

### Problemas encontrados ou pendências

- A tela de espessura ainda precisa de conferência visual após reiniciar o Electron; TypeScript e diff foram aprovados.
- A moldura e a barra do Windows podem permanecer no modo de desenvolvimento; o foco do quiosque é o aplicativo empacotado.
- O repouso usa padrão de 30 minutos via variável de ambiente e ainda não possui seletor na UI.
- Falta homologar o comportamento completo em mini PC com câmera, touch, monitor retrato, segunda tela, suspensão e reconexão.
- A escala física e o perfil calculado precisam ser conferidos com outros tamanhos de lente e ângulos de rotação.

### Próximos passos

1. Reiniciar `npm run dev:electron` e revisar a tela de espessura em 0°, 90° e outros ângulos. Consumo baixo.
2. Testar o ciclo abertura → experiência → abertura → repouso → despertar. Consumo baixo.
3. Homologar câmera e tela retrato no equipamento real. Consumo alto.
4. Adicionar configuração visual do tempo de repouso, se necessário. Consumo médio.
5. Só depois gerar o instalador Neosmart e iniciar o piloto. Consumo médio.

### Ideias futuras

- Painel de diagnóstico da tela do cliente com estado atual, monitor detectado, orientação, escala e ação de reativação.
- Opções de repouso Nunca/10/15/30/60 minutos.
- Teste automatizado de equivalência visual entre a vista 3D, o perfil calculado e o mapa frontal.

## 23/07/2026 — UX em duas telas, espessura e continuidade

### O que foi feito

- A tela do cliente passou a permanecer aberta no Electron. Sem experiência
  ativa, apresenta `public/abertura.mp4` em loop; ao sair de qualquer tela,
  retorna automaticamente à abertura.
- Implementado repouso preto por inatividade, com padrão de 30 minutos e
  despertar por interação, desbloqueio, retorno da suspensão ou reconexão do
  monitor. Corrigido o uso para `powerMonitor.getSystemIdleTime()`.
- Câmera ou demonstração agora também definem o objetivo da tela do cliente.
  Mensagens operacionais foram removidas da apresentação; falhas ficam nos
  logs e diagnósticos do operador.
- Fullscreen/kiosk ficou restrito ao aplicativo empacotado. Em desenvolvimento,
  moldura e barra do Windows podem permanecer visíveis.
- Corrigido o acionamento da câmera no Visagismo e sua publicação na segunda
  tela. Em retrato, vídeo e coordenadas foram ajustados e o carrossel de
  armações foi movido para perto do topo.
- Compactado o menu exibido após a leitura do Campo Visual para evitar
  sobreposição e remontagem dos botões.
- Na espessura, a apresentação ampliada tornou-se o padrão e tamanho real
  permaneceu opcional. Os índices disponíveis são `1.49`, `1.56`, `1.59`,
  `1.67` e `1.74`, com `1.56` inicial.
- Reorganizada a tela de espessura em retrato: borda física e perfil calculado
  ficam acima; a lente frontal, explicativa, fica abaixo.
- As três representações agora usam a mesma escala em pixels por milímetro.
  Removidas a ampliação duplicada da lente frontal e a redução artificial da
  câmera 3D. Os canvases usam 640 px e a geometria é recentralizada após cada
  giro sem mudar a escala.
- Rastreado no SQLite o atendimento indevido das 10h45. O fluxo agora separa
  explicitamente `new` e `resume`; retomada sem UUID falha sem criar sessão, e
  a importação de sessão remota preserva UUID, horário e cliente.
- Corrigido o dropdown de atendimentos: a sessão local não apaga mais o cliente
  remoto, clientes provisórios locais também fornecem nome e a seleção abre
  diretamente o menu de experiências.
- Validação concluída com 31 testes, typecheck, lint direcionado e
  `git diff --check`.
- Atualizado `CURRENT_STATUS.md` e os três documentos compartilhados apontados
  por `NEOSMART_CONTEXT_START.md`. Não houve deploy.

### Problemas encontrados ou pendências

- O atendimento indevido das 10h45 permanece preservado; não foi apagado nem
  mesclado.
- A equivalência visual das três representações de espessura ainda deve ser
  conferida no Electron autenticado em vários ângulos, centros ópticos, índices
  e tamanhos de armação.
- O ciclo completo de câmera, touch, monitor retrato, repouso, suspensão,
  reconexão e retorno à abertura ainda precisa de homologação no mini PC.
- Retomada online/offline e outbox ainda precisam ser testadas com uma sessão
  existente apenas no servidor.

### Próximos passos

1. Reiniciar o Electron e testar “Continuar atendimento” com uma sessão que
   possua cliente, confirmando UUID, horário e nome no dropdown. Consumo baixo.
2. Conferir a tela de espessura em 0°, 90°, centro óptico deslocado e armação
   máxima, sem cortes ou diferença aparente de escala. Consumo baixo.
3. Validar abertura, experiência, retorno, repouso e despertar nas duas telas
   reais. Consumo médio.
4. Testar queda/retorno da internet e sincronização da outbox sem duplicação.
   Consumo alto.
5. Gerar o instalador somente depois dessas validações e da homologação do modo
   appliance. Consumo médio.

### Ideias futuras

- Teste visual automatizado que compare a extensão aparente das três
  representações de lente em vários ângulos.
- Diagnóstico de sessão mostrando, apenas ao suporte, UUID local/remoto, estado
  da outbox e origem do nome do cliente.
- Configuração de repouso com opções Nunca/10/15/30/60 minutos.

## 24/07/2026 — diagnóstico de fluxos e coerência da interface

### O que foi feito

- Revisados integralmente o documento canônico de decisões, o contexto de
  produto, o status da sandbox do heatmap, o `CURRENT_STATUS.md`, o README da
  Neosmart e a memória anterior.
- Executado debug direcionado a fluxo, botões, navegação, estados de
  carregamento, segunda tela e coerência entre a ação do operador e o resultado
  apresentado, sem auditoria de segurança.
- Reproduzido no renderer publicado que a perda de autorização envia a Torre
  para `/login`, rota inexistente na Neosmart, resultando em página 404.
- Confirmado no Electron que “Abrir teste na segunda tela” referencia
  `customerUrl` antes de sua declaração quando a janela persistente do cliente
  já existe; como essa janela é aberta na inicialização, o fluxo de diagnóstico
  pode falhar justamente no uso normal.
- Confirmado que o helper compartilhado da tela do cliente não aguarda o
  resultado assíncrono do Electron. Assim, vários botões mudam para “Voltar à
  abertura” mesmo sem confirmação de que a experiência foi aberta.
- Identificados comandos por atraso fixo de 700 ms em Medidas e Seu Jeito de
  Olhar, sem o handshake de cliente pronto já existente no Visagismo. Em
  carregamento frio, o primeiro comando pode ser perdido.
- Identificado uso de `useTransition` para operações assíncronas no React 18 no
  menu de sessões e em Medidas. O estado visual de carregamento não acompanha
  todo o `await`, deixando botões disponíveis e a lista de retomada sem feedback
  confiável.
- Identificado que o Campo Visual libera “Continuar para avaliação” e “Encerrar
  atendimento” antes da confirmação de persistência do mapa; em erro de
  salvamento, não existe ação de tentar salvar novamente.
- Identificados erros silenciosos ao criar/retomar sessão no menu: experiências
  podem apenas exibir a mensagem de mock, e Espessura pode não reagir, em vez de
  mostrar o erro real.
- Confirmada a causa de novas sessões após entrar em um atendimento existente:
  AR, Opti Fog, Polarizadas, Seu Jeito de Olhar e Comparativo de Campos removem
  o UUID ao abrir e ao voltar. O menu retorna com `menu=informacoes`, mas sem
  `session`; nessa combinação, `TowerWelcomeMock` inicializa
  `selectedAction = 'new'`. A experiência seguinte chama a criação em modo
  `new` e gera outro UUID. Espessura é a única Informação Útil que preserva a
  sessão atual.
- Conferido o contrato central do MB Optical: com `sessionId`, ele localiza e
  atualiza a sessão ativa; uma nova linha só é inserida quando o UUID não é
  enviado. A duplicação, portanto, nasce na perda de contexto da navegação da
  Neosmart, não no endpoint de sessões.
- Confirmada identidade visual residual de Gestão Ótica/Ótica Pro no título,
  manifesto, tela inicial e cabeçalho, divergindo do produto Neosmart.
- Validação técnica concluída sem alterações funcionais: typecheck aprovado, 31
  testes aprovados, build de produção aprovado e `git diff --check` aprovado.
- As sete frentes do diagnóstico foram corrigidas na Neosmart. Todas as
  Informações Úteis agora criam ou retomam a sessão antes de abrir e carregam o
  mesmo UUID na ida e no retorno; o caminho retomada → informação → menu não
  muda mais silenciosamente para `new`.
- O diagnóstico da segunda tela passou a construir `customerUrl` antes de
  reutilizar a janela persistente. O helper compartilhado agora aguarda o
  resultado do Electron tanto ao abrir quanto ao fechar, e os estados locais só
  mudam após confirmação de sucesso.
- Medidas e Seu Jeito de Olhar passaram a usar `clientReady`; foram removidos os
  atrasos fixos de 500/700/800 ms que podiam perder o primeiro comando em
  carregamento frio.
- O menu e a apresentação das medidas passaram a usar estados assíncronos
  explícitos. Botões ficam bloqueados durante a operação e falhas de
  criação/retomada são exibidas ao operador em vez de cair em mock ou silêncio.
- O Campo Visual agora aguarda a confirmação de persistência antes de liberar
  avaliação ou encerramento, mostra o estado de gravação e oferece “Tentar
  salvar novamente” em caso de erro.
- Todas as rotas operacionais deixaram de enviar falhas de sessão para o
  `/login` inexistente. A recuperação retorna para
  `/torre/inicial?reason=session`, com orientação visível para reativar a Torre.
  A página de falha ao preparar o Campo Visual também ganhou retry e retorno.
- Título, manifesto, tela inicial, diagnóstico, cabeçalho e janela Electron
  foram alinhados à identidade Torre Neosmart. A demonstração Opti Fog também
  deixou de manter rótulos de comparação quando a comparação está desligada.
- Adicionadas regressões para continuidade do UUID em todas as Informações
  Úteis, handshake sem atraso fixo, persistência antes da progressão,
  recuperação sem `/login` e inicialização do diagnóstico da segunda tela.
  O `lint:tower` foi reparado para apontar apenas para arquivos existentes e
  passou a cobrir os componentes operacionais alterados.
- Validação final: typecheck aprovado, 36 testes aprovados, lint sem erros
  (16 avisos antigos), build de produção aprovado, `git diff --check` aprovado
  e smoke local do redirecionamento/aviso de recuperação aprovado.

### Problemas encontrados ou pendências

- As correções de código e os testes automatizados estão concluídos, mas o
  fluxo autenticado de duas telas ainda deve ser homologado no Electron e no
  mini PC real, principalmente câmera, touch, monitor retrato, carregamento
  frio, suspensão e reconexão.
- Os 16 avisos do lint já existiam em componentes grandes do heatmap,
  avaliação e visagismo; não impedem compilação nem testes, mas merecem limpeza
  separada para que o gate possa futuramente usar tolerância zero a warnings.
- O working tree da Neosmart continua contendo as alterações locais anteriores
  em `README.md`, `electron/main.cjs`, `package.json` e
  `tests/electron-security.test.mjs`; elas foram preservadas e integradas sem
  reset ou sobrescrita.

### Próximos passos

1. Homologar no Electron autenticado a retomada de uma sessão existente
   passando por cada Informação Útil e conferir que o UUID não muda. Consumo
   médio.
2. Testar no mini PC o primeiro clique de Medidas e Seu Jeito de Olhar com
   carregamento frio, além de abrir/fechar a segunda tela repetidamente. Consumo
   médio.
3. Simular falha e retorno da API ao concluir o Campo Visual, confirmando o
   bloqueio, o retry e a liberação após salvar. Consumo médio.
4. Repetir o ciclo câmera, touch, monitor retrato, repouso, suspensão e
   reconexão no equipamento real. Consumo alto.
5. Limpar os avisos antigos do lint em uma mudança separada e então considerar
   `--max-warnings=0`. Consumo médio.

### Ideias futuras

- Um estado compartilhado da tela cliente, confirmado pelo Electron, para que
  todos os botões usem `abrindo`, `apresentando`, `abertura` e `erro` em vez de
  booleanos locais.
- Testes com atraso artificial de carregamento e rede para capturar comandos
  perdidos e cliques duplicados.
- Um painel de recuperação do atendimento que mostre, ao operador, sessão,
  persistência do mapa, resultado de medidas e situação da sincronização antes
  de permitir avançar.

## 25/07/2026 — contexto do cliente, cadastro na Espessura e ideia de página compartilhável

### O que foi feito

- Na Espessura de lentes, quando a sessão já possui cliente, a busca deixou de
  ficar permanentemente visível. A UI mostra o cliente da sessão com nome e
  telefone e oferece `Trocar cliente` para reabrir a pesquisa.
- Foi adicionado `Cadastrar cliente rapidamente` dentro da busca da Espessura,
  com nome completo e celular.
- O cadastro reutiliza o fluxo operacional do Campo Visual por meio de
  `createOperationalTowerCustomer`, mantendo o caminho web e o caminho
  local-first do Electron.
- O novo cliente é vinculado à sessão atual. Antes de salvar a receita,
  `resolveOperationalTowerCustomer` confirma se um cliente provisório já
  recebeu ID remoto; se ainda não recebeu, a UI informa a pendência e não
  envia uma receita que poderia falhar ou ficar sem vínculo.
- A Espessura passou a ler a receita do snapshot compartilhado da sessão e a
  recuperar, como fallback, a receita registrada na avaliação do Campo Visual
  quando sessões antigas ainda possuem snapshot zerado. Cada abertura do modal
  reinicializa cliente e receita a partir do contexto persistido, sem herdar
  uma edição cancelada.
- Foi pesquisado o cadastro existente do Campo Visual. Não foi encontrado
  erro estrutural no fluxo; o cuidado necessário era não copiar a UI sem o
  tratamento do cliente provisório.
- Foram executados `npm run typecheck` e 29 testes de
  `tests/mb-optical-contracts.test.mjs`, todos aprovados.
- Foi discutida, sem implementação, uma futura página compartilhável do
  cliente: selecionar dados da sessão, gerar um QR Code e permitir o download
  de um PDF.

### Problemas encontrados ou pendências

- As alterações desta data estão no working tree da Neosmart e ainda não
  foram commitadas, enviadas ou publicadas.
- O cadastro novo precisa ser testado na Torre com internet disponível e,
  separadamente, com sincronização pendente, para confirmar a mensagem e a
  retomada após o ID remoto chegar.
- A página do cliente, a seleção dos dados compartilháveis, o token de acesso,
  o QR Code e o PDF ainda não existem no fluxo operacional.

### Próximos passos

1. Fazer commit e deploy web das alterações da Espessura e testar cadastro,
   troca de cliente e salvamento da receita na sessão real. Consumo baixo.
2. Testar a criação de cliente provisório no Electron com queda/retorno de
   conexão e confirmar que a receita só é salva após sincronização. Consumo
   médio.
3. Homologar a UI em 1024 × 768, especialmente a área de cadastro rápido e o
   modal de receita. Consumo baixo.
4. Especificar os dados que podem ser compartilhados com o cliente antes de
   implementar a página pública. Consumo médio.

### Ideias futuras

- Página pública da sessão com token opaco, expirável e revogável, sem expor o
  UUID interno.
- Botão na Torre para selecionar resumo, receita, mapa, indicações, armação e
  comparativos antes de gerar o QR Code.
- Geração de PDF para download pelo cliente, liberada somente após os dados
  estarem sincronizados.
