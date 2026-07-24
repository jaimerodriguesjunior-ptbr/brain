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
- Confirmado que várias Informações Úteis não preservam o UUID ao voltar, apesar
  de poderem ser abertas dentro de um atendimento retomado. Espessura preserva,
  mas AR, Opti Fog, Polarizadas, Seu Jeito de Olhar e Comparativo de Campos não.
- Confirmada identidade visual residual de Gestão Ótica/Ótica Pro no título,
  manifesto, tela inicial e cabeçalho, divergindo do produto Neosmart.
- Validação técnica concluída sem alterações funcionais: typecheck aprovado, 31
  testes aprovados, build de produção aprovado e `git diff --check` aprovado.

### Problemas encontrados ou pendências

- Corrigir primeiro o diagnóstico da segunda tela e o redirecionamento para o
  `/login` inexistente, pois ambos bloqueiam fluxos inteiros.
- A suíte atual valida contratos e persistência, mas não cobre os handshakes,
  estados de carregamento, retorno de navegação ou a correspondência entre o
  rótulo dos botões e o estado real da segunda tela.
- O fluxo completo autenticado ainda precisa ser repetido no Electron e no mini
  PC após as correções, incluindo carregamento frio das experiências.
- O working tree da Neosmart já continha alterações em `README.md`,
  `electron/main.cjs`, `package.json` e `tests/electron-security.test.mjs`;
  elas foram preservadas e não foram modificadas por este diagnóstico.

### Próximos passos

1. Corrigir `openCustomerDisplayTest`, retornar/aguardar resultados reais do
   helper da tela cliente e exibir falhas no painel do operador. Consumo baixo.
2. Substituir o redirecionamento para `/login` por recuperação própria da
   Neosmart e adicionar teste de navegação. Consumo baixo.
3. Padronizar handshake `clientReady` em Medidas e Seu Jeito de Olhar. Consumo
   médio.
4. Trocar os `useTransition` assíncronos por estados explícitos e mostrar erros
   reais de criação/retomada de sessão. Consumo médio.
5. Bloquear avaliação/encerramento até o mapa estar salvo e oferecer retry de
   persistência. Consumo médio.
6. Preservar o UUID ao entrar e sair de todas as Informações Úteis e alinhar a
   identidade visual para Neosmart. Consumo baixo.
7. Criar testes de fluxo para segunda tela, carregamento frio, retomada e falha
   de API; depois repetir o smoke test no mini PC. Consumo alto.

### Ideias futuras

- Um estado compartilhado da tela cliente, confirmado pelo Electron, para que
  todos os botões usem `abrindo`, `apresentando`, `abertura` e `erro` em vez de
  booleanos locais.
- Testes com atraso artificial de carregamento e rede para capturar comandos
  perdidos e cliques duplicados.
- Um painel de recuperação do atendimento que mostre, ao operador, sessão,
  persistência do mapa, resultado de medidas e situação da sincronização antes
  de permitir avançar.
