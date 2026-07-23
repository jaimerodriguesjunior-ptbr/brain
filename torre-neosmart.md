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
