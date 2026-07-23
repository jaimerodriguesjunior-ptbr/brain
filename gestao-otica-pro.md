# DiÃ¡rio - 20/07/2026

## O que foi feito

- Reorganizado o menu inicial da Torre: â€œContinuar atendimentoâ€ passou a usar o mesmo menu de experiÃªncias de â€œNovo atendimentoâ€, com seleÃ§Ã£o das seÃ§Ãµes abertas por data, hora e cliente.
- Corrigidos avisos de uso sÃ­ncrono de `cookies()` nas rotas do Next/Supabase.
- ReforÃ§ada a navegaÃ§Ã£o do Electron para impedir que o touch caia no login ou em rotas administrativas.
- Melhorado o fluxo do PIN administrativo, exibindo a troca de PIN somente quando solicitada.
- Implementada a persistÃªncia das aprovaÃ§Ãµes de cÃ¢mera, touch e tela do cliente no SQLite local, com outbox para sincronizaÃ§Ã£o no Supabase.
- Criada e aplicada a migration de aprovaÃ§Ãµes de hardware no Supabase, vinculada ao ativo fÃ­sico da Torre.
- A faixa de status da Torre deixou de usar â€œTela cliente conectadaâ€ e â€œCÃ¢mera prontaâ€ fixos; agora consulta o estado real do Electron e das aprovaÃ§Ãµes.
- Removidos da tela do cliente os alertas de medidas que poderiam prejudicar a venda. Esses alertas continuam disponÃ­veis para o funcionÃ¡rio e sÃ£o gravados no resultado tÃ©cnico.

## Problemas encontrados

- As imagens frontal e de perfil da tela de Medidas ainda ficam somente na memÃ³ria da pÃ¡gina. Ao sair e retornar Ã  mesma seÃ§Ã£o, os resultados numÃ©ricos permanecem no fluxo, mas as imagens desaparecem.
- O Electron precisa ser reiniciado apÃ³s alteraÃ§Ãµes no `preload.cjs`; atualizar apenas a pÃ¡gina nÃ£o carrega novas funÃ§Ãµes IPC.
- A tela de Medidas ainda precisa de um rascunho local para recuperar imagens e estado parcial da seÃ§Ã£o.

## PrÃ³ximos passos

1. Persistir no SQLite os rascunhos de Medidas vinculados ao `tower_session_id`.
2. Recuperar automaticamente imagens frontal e de perfil ao retornar Ã  seÃ§Ã£o ativa.
3. Definir a polÃ­tica de descarte das imagens ao concluir ou descartar o atendimento.
4. Validar o fluxo completo no Electron com duas telas, cÃ¢mera e touch reais.

## Ideias futuras

- Manter fotos de Medidas somente no equipamento, protegidas e fora do Supabase por padrÃ£o.
- Exibir no funcionÃ¡rio um histÃ³rico resumido das revisÃµes e correÃ§Ãµes feitas nas medidas.
- Criar um estado visual de sincronizaÃ§Ã£o da Torre no menu principal.

## Ideia futura — armações 3D para espessura

A tentativa inicial com `armacao.dae` não produziu resultado aceitável porque usou a malha original dos vidros como base. Para uma versão futura, exportar do SketchUp a armação sem lentes renderizadas, com grupos ou contornos-guia nomeados `encaixe_OD` e `encaixe_OE`, além de referências de plano frontal, eixo de profundidade e centro óptico. Esses contornos serão usados como estrutura para reconstruir no Three.js uma lente sólida com frente curva, traseira recuada pela espessura calculada e parede lateral fechada. A ideia permanece futura e exige calibração geométrica explícita.

# Diario - 21/07/2026

## O que foi feito

- Corrigido o fluxo de pedido de chave Pix no WhatsApp: a chave e enviada e a conversa entra em pausa para atendimento humano.
- Corrigido o radar operacional do WhatsApp para atualizar o contador de pendencias automaticamente a cada 30 segundos, sem depender da abertura da central operacional.
- Corrigida a consulta do radar para carregar expires_at antes de classificar handoffs; validado com a pendencia real da Regiana na loja 1.
- Ajustados os campos de cilindro das OSs na Vendas Experimental para assumirem o sinal negativo ao receber foco; por exemplo, 025 passa a -0,25.
- A Torre passou a autorizar a tela de avaliacao pela credencial do dispositivo pareado e pela loja, sem exigir sessao humana do dashboard; a busca de clientes e as recomendacoes de lentes tambem passaram a respeitar esse contexto operacional e o store_id.
- Adicionadas protecoes de sincronizacao da Torre contra eventos repetidos, conflitos de payload e duplicacao de clientes por telefone ou nome, incluindo migration para o mapeamento entre cliente local e cliente remoto.
- Criada a configuracao remota de catalogos globais por loja: a sessao comercial da Torre pode consultar versoes publicadas e ativar uma versao para a loja, com exibicao de familias, ofertas e tratamentos.
- Disponibilizada na configuracao remota a edicao das prioridades comerciais das sugestoes de lentes, incluindo perfil de investimento, adocao tecnologica, prioridade estetica, laboratorios e marcas; os valores recebidos passam por saneamento.
- Melhorado o fluxo da avaliacao para abrir a tela do cliente com a recomendacao, compartilhar o estado via localStorage e canal de mensagens, e interromper a camera antes da apresentacao.
- Corrigida a atualizacao do dashboard e do menu operacional depois do recebimento de parcela, usando refresh e evento local para atualizar os alertas.
- Ajustada a busca de clientes/vendas para o caso pesquisado de Joao e incluida a acao para destruicao de duplicatas.
- Concluida a segunda fatia do Passo 9 da Torre: criado endpoint autenticado por dispositivo para gerar snapshot versionado da configuracao da interface, catalogos ativos e prioridades comerciais.
- O Electron passou a baixar esse snapshot no pareamento, na inicializacao, sob demanda e a cada cinco minutos; o conteudo fica cifrado pelo safeStorage e isolado no SQLite por loja e dispositivo.
- A tela inicial da Torre passou a aplicar a configuracao local quando disponivel. Foram adicionados testes do cache cifrado, do IPC e do encadeamento entre as funcoes SQL v3 e v2.
- Iniciado o Passo 10: configurado o empacotamento Windows x64 com electron-builder e instalador NSIS por maquina, usando a URL HTTPS de producao, modo kiosk e inicializacao automatica por padrao.
- Gerado o instalador Torre-MB-Optical-Setup-0.1.0-x64.exe e validada a abertura do executavel empacotado em smoke test no Windows de desenvolvimento.
- Criado o roteiro TOWER_WINDOWS_INSTALLATION.md com geracao, instalacao, pareamento da loja 7, homologacao fisica, preservacao dos dados locais e assinatura futura.
- A Torre foi separada do MB Optical e renomeada para Neosmart. Foi criado o repositorio independente `G:\projetos\torre-neosmart`, com Next 14.2.35, React 18.2.0, Electron 43.1.1, identidade de pacote propria e sem os modulos administrativos do MB Optical.
- O Electron da Neosmart passou a tratar separadamente a origem do renderer (`NEOSMART_RENDERER_URL`) e a origem das APIs centrais (`MB_OPTICAL_API_URL`). A credencial permanente permanece no processo principal e o renderer recebe apenas uma sessao curta em cookie HTTP-only.
- Foram criados contratos autenticados e versionados `/api/tower/v1/web/*` no MB Optical. Busca de clientes, contexto, criacao/retomada, listagem, vinculo de cliente, vinculo de avaliacao, receita, conclusao e descarte de sessao passaram a ser consumidos pela Neosmart via HTTP.
- O repositorio Neosmart ficou nos commits `505e4d9`, `a614bc4`, `4dbca02` e `fa043f4`. No MB Optical, os contratos ficaram em `99d110d` e `02f4653`.
- Investigados seis deployments consecutivos com erro na Vercel. O ultimo deployment funcional, commit `8760a1d`, usava Next 14.2.33; o primeiro erro, commit `8653076`, usava Next 15.5.20. Ambos terminavam o build, mas o Next 15 falhava durante `Deploying outputs`.
- O MB Optical foi restaurado para Next 14.2.35, patch seguro da linha 14, e a fabrica de Supabase voltou ao contrato sincrono de cookies compativel. Typecheck, 25 testes e build passaram. A restauracao ficou em `eb2cc3f`; o commit vazio `9f52470` repetiu o deploy, que chegou a `Ready`.
- Criada a branch local `backup/mboptical-before-next14-rollback-2026-07-21` antes da regressao, preservando integralmente o estado anterior.
- O `vercel build` local no Windows ainda acusou `Unable to find lambda for route: /demo/menu/atendimento`, mas isso foi confirmado como falso negativo local: a rota existia no ultimo deploy funcional e a publicacao remota com Next 14 concluiu normalmente.

## Problemas encontrados ou pendencias

- O aviso de uso sincronico de cookies no dashboard da loja continua pendente de migracao da fabrica legada createClient() para createAsyncClient().
- A validacao visual do radar e dos campos de grau ainda precisa ser feita no ambiente com mensagens reais e uma OS aberta.
- A suspeita de divergencia entre apply_tower_device_sync_event_v2 e v3 era um falso negativo: a v3 trata hardware e delega os demais eventos para a v2 corrigida.
- As migrations da etapa foram informadas como aplicadas no ambiente remoto. Ainda nao foi validado o fluxo completo da Neosmart com dispositivo pareado real.
- O snapshot local guarda configuracao e identidade das versoes ativas, mas nao todas as linhas do catalogo nem o motor de recomendacao; operacao integral sem rede depende do empacotamento previsto no Passo 10.
- A desativacao de catalogo existe no backoffice, mas ainda nao esta exposta na configuracao comercial remota da Torre.
- O instalador do primeiro piloto ainda nao possui assinatura de codigo e pode exibir alerta do Windows SmartScreen.
- A cadeia de ferramentas de desenvolvimento do empacotador possui alertas de dependencia; o audit das dependencias de producao nao encontrou alertas altos ou criticos.
- O MB Optical voltou a publicar com status `Ready`. As novas APIs estao no codigo publicado, mas o fluxo integrado com um deploy separado da Neosmart ainda precisa ser testado.
- O instalador antigo com nome Torre MB Optical deixou de ser candidato ao piloto depois da separacao. Um novo instalador Neosmart deve ser gerado somente ao final da nova validacao.
- Medidas, heatmap, ativos e possiveis leituras compartilhadas de catalogo ainda precisam ser inventariados e migrados para contratos HTTP antes de considerar a separacao concluida.

## Proximos passos

1. Validar o radar com uma nova mensagem que gere handoff humano.
2. Testar os campos de esferico e cilindro em todas as paginas filhas de OS.
3. Migrar os usos restantes de createClient() sincronico no servidor.
4. Confirmar as migrations no ambiente remoto e testar repeticao, conflito e dependencia de cliente em um dispositivo real.
5. Validar no Electron o download, a recuperacao do cache e a aplicacao da configuracao apos queda e retorno da internet.
6. Definir no Passo 10 o recorte local necessario a recomendacao totalmente offline, separado do instalador do primeiro piloto conectado.
7. Publicar o backend atualizado e instalar o executavel no mini PC da Torre para homologar camera, touch, segunda tela, reinicio e sincronizacao real.
8. Adquirir e configurar certificado de assinatura de codigo antes da distribuicao comercial.

### Ordem de retomada em 22/07/2026

1. Fazer smoke test curto do MB Optical restaurado: login, abertura de loja, atendimento, clientes e uma area critica. Consumo baixo.
2. No repositorio Neosmart, confirmar o HEAD `fa043f4` e repetir `npm install`, typecheck, 19 testes e build como linha de base. Consumo baixo.
3. Inventariar os acessos restantes ao Supabase e migrar medidas, heatmap e ativos, um dominio por vez, para APIs HTTP v1. Consumo alto.
4. Auditar que o Neosmart nao contem service role, chave administrativa ou acesso amplo ao banco. Consumo medio.
5. Criar o projeto Vercel exclusivo da Neosmart e configurar renderer e API em origens separadas. Consumo medio.
6. Repetir os testes integrados afetados pela separacao: pareamento, sessao, cliente provisorio, medidas, outbox, queda/retorno da internet e reconciliacao. Consumo alto.
7. So depois retomar o novo Passo 10, gerar o instalador Neosmart e homologar camera, touch, segunda tela, kiosk, reinicio e persistencia no mini PC da Loja 7. Consumo alto.

Nao e necessario recomecar todos os testes ou reconstruir casos existentes. Os testes automatizados devem ser reexecutados como regressao depois de cada lote. Os testes manuais de integracao e hardware precisam ser repetidos porque o executavel, a origem do renderer e a fronteira entre os repositorios mudaram.

## Ideias futuras

- Substituir o refresh periodico do radar por atualizacao em tempo real via evento ou polling dedicado de menor custo.
- Criar uma rotina automatizada de auditoria para comparar a versao do catalogo/configuracao no servidor com a versao aplicada em cada Torre.
- Criar um contrato compartilhado publicavel ou gerado para reduzir divergencias de tipos entre as APIs MB Optical e os clientes Neosmart, sem voltar a acoplar os repositorios.

# Diario - 22/07/2026

## Atualização Neosmart — experiência da Torre

### O que foi feito

- A continuidade de atendimento foi corrigida na Neosmart: primeiro o operador seleciona o atendimento aberto e depois escolhe a experiência, preservando o UUID da sessão.
- A tela do cliente passou a ser persistente no Electron. Ao iniciar, acordar, reconectar o monitor ou ficar sem objetivo, exibe `public/abertura.mp4` em loop.
- Após 30 minutos de inatividade, a tela do cliente entra em repouso preto; uma nova interação, desbloqueio ou retorno da suspensão restaura a abertura. Durante experiências ativas, o repouso não é aplicado.
- Os controles das experiências deixaram de tratar a tela como uma janela descartável: os textos passaram a indicar apresentação ao cliente ou retorno à abertura.
- O erro do monitor de inatividade foi corrigido para usar `powerMonitor.getSystemIdleTime()`.
- O Visagismo em monitor retrato passou a preencher a tela com `object-cover`, com o mapeamento de coordenadas ajustado para manter rosto, armação e marcadores alinhados.
- O carrossel de armações durante a pesquisa foi movido para perto do topo para não competir visualmente com o rosto.
- A tela de espessura das lentes foi reorganizada para retrato: as vistas física e de perfil calculado recebem prioridade, e a lente frontal fica abaixo como explicação.
- As vistas da espessura passaram a compartilhar pixels por milímetro; a área frontal foi ampliada para um canvas quadrado de `520 × 520` para permitir rotação sem cortes.

### Problemas encontrados ou pendências

- As alterações de espessura e apresentação em retrato foram validadas por TypeScript e diff, mas ainda precisam de uma nova conferência visual no Electron após reiniciar o processo.
- A vista física usa Three.js e o perfil usa SVG; a escala foi alinhada por pixels por milímetro, mas a homologação deve confirmar a equivalência visual em diferentes graus, índices e tamanhos de armação.
- A janela do Electron em desenvolvimento ainda pode exibir moldura/barra do Windows; o usuário decidiu manter isso por enquanto. O modo quiosque continua sendo prioridade do aplicativo empacotado.
- A configuração do repouso está com padrão de 30 minutos por variável de ambiente; ainda não existe controle visual de configuração para o operador.
- Câmera, touch, segunda tela, orientação retrato, inicialização, suspensão e reconexão ainda não foram homologados no mini PC real.

### Próximos passos

1. Reiniciar o Electron e conferir visualmente a tela de espessura em retrato, especialmente a rotação a 90 graus e a igualdade entre as três vistas. Consumo baixo.
2. Validar o fluxo da tela persistente com duas telas reais, incluindo abertura, experiência, retorno à abertura, repouso e despertar. Consumo médio.
3. Homologar câmera em retrato e confirmar o alinhamento da armação em posições variadas do rosto. Consumo médio.
4. Criar, se necessário, uma configuração visual para o tempo de repouso da tela do cliente. Consumo médio.
5. Gerar o instalador Neosmart somente depois da homologação física e dos testes integrados. Consumo médio.

### Ideias futuras

- Exibir na configuração da Torre o estado da tela do cliente e um botão de teste/reativação.
- Permitir as opções Nunca, 10, 15, 30 e 60 minutos para repouso, mantendo atendimento ativo sempre protegido contra repouso.
- Criar uma validação visual automatizada para comparar a escala das representações de espessura.

## O que foi feito

- Publicados os ajustes de experiencia da Neosmart e do MB Optical, incluindo
  narrativas de IA com fallback limpo, comparativos AR/polarizado e exibicao do
  estado real de camera, touch e tela cliente.
- Corrigida a continuidade de atendimento: ao retornar ao menu com uma sessao
  existente, o UUID da URL agora sincroniza o estado interno e a experiencia
  atualiza a mesma sessao em vez de criar outra.
- Atualizados os tres documentos-base de contexto no MB Optical e registrados
  os estados publicados da Neosmart.
- Testes atuais aprovados: 26 no MB Optical e 30 na Neosmart; typecheck e
  builds aprovados nos dois repositorios.

## Problemas encontrados ou pendencias

- Duas sessoes duplicadas criadas antes da correcao permanecem no banco. Elas
  nao foram apagadas nem mescladas; a reconciliacao precisa de decisao
  explicita para preservar o historico correto.
- O instalador Windows Neosmart e a homologacao completa no mini PC ainda nao
  foram concluídos.
- O inventario de actions legadas e o roteiro de instalacao ainda precisam ser
  mantidos alinhados com o estado publicado.

## Proximos passos

1. Reexecutar o smoke test integrado online e offline no Electron (consumo de
   IA medio).
2. Homologar camera, touch, segunda tela, kiosk, reinicio e outbox no mini PC
   da Loja 7 (consumo de IA alto).
3. Gerar o instalador Neosmart somente depois das validacoes (consumo de IA
   medio).
4. Decidir a reconciliacao das duas sessoes antigas (consumo de IA baixo).
5. Classificar actions legadas por consumidor antes de qualquer remocao
   (consumo de IA medio).

## Ideias futuras

- Criar uma auditoria automatica que compare sessoes, resultados e eventos de
  outbox para detectar duplicacoes antes de chegarem ao operador.
- Exibir no menu um estado resumido de sincronizacao da sessao atual.

# Diário - 23/07/2026

## O que foi feito

- Atualizados os documentos de contexto compartilhado da Neosmart no MB
  Optical: `TOWER_DEVELOPMENT_DECISIONS.md`,
  `TOWER_AND_TABLET_VISION_CONTEXT.md` e
  `HEATMAP_HEAD_SANDBOX_STATUS.md`.
- Registrados os ajustes do protótipo em duas telas: abertura persistente,
  repouso, câmera e composição em retrato, fullscreen somente no empacotado e
  limpeza das mensagens operacionais da tela do cliente.
- Documentada a apresentação de espessura com prioridade para as bordas,
  canvas de 640 px e uma única escala em pixels por milímetro para vista física,
  perfil calculado e lente frontal.
- Documentado o segundo endurecimento da continuidade de sessões: modos
  explícitos `new` e `resume`, retomada obrigatoriamente vinculada a UUID e
  preservação do horário original ao importar uma sessão remota no SQLite.
- Registrada a correção do dropdown para preservar o cliente retornado pelo MB
  Optical durante a mesclagem com a sessão local.

## Problemas encontrados ou pendências

- Uma sessão indevida de espessura criada às 10h45 em 23/07/2026 permanece no
  histórico. Nenhum dado foi removido ou reconciliado automaticamente.
- A integração online/offline e a outbox ainda precisam de homologação completa
  no mini PC, especialmente ao retomar sessões existentes apenas no servidor.
- Não houve alteração de contrato nem deploy do MB Optical nesta etapa.

## Próximos passos

1. Testar a retomada de uma sessão remota com cliente no Electron e confirmar
   que o MB Optical mantém UUID e `started_at` originais. Consumo médio.
2. Decidir a reconciliação das sessões indevidas preservadas. Consumo baixo.
3. Homologar queda e retorno da internet, retries e ausência de duplicação na
   outbox. Consumo alto.
4. Manter os contratos `/api/tower/v1/web/*` como autoridade central e revisar
   qualquer acesso administrativo residual antes do piloto. Consumo médio.

## Ideias futuras

- Criar no MB Optical uma auditoria de sessões que destaque UUIDs criados ou
  atualizados por retomadas inconsistentes, sem apagar dados automaticamente.
- Expor no suporte uma comparação segura entre sessão local, sessão remota e
  eventos da outbox para facilitar reconciliações.

