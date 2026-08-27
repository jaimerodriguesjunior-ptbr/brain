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

# Diário - 24/08/2026

## O que foi feito

- A Central Diária deixou de exibir como atenção operacional pedidos sem atualização de chegada há 120 horas e datas fora de sequência; OS sem data prometida e múltiplas OS abertas permanecem como verificações relevantes.
- Os IDs antigos desses alertas foram tratados como descontinuados para não aparecerem como pendências resolvidas no lifecycle.
- A Loja 1 foi regenerada e passou a mostrar na operação somente os alertas ainda considerados relevantes: lente não pedida ao laboratório, lente não chegada até a data prometida e venda de lente sem OS.
- Os quatro indicadores retirados também foram excluídos da entrada enviada à IA, evitando que reapareçam na narrativa por engano.
- Corrigida a regra operacional: o prazo de 24 horas começa quando a lente chega à loja e termina quando a montagem local é registrada. Pedido antigo no laboratório não é, por si só, alerta de montagem.
- Criado alerta separado para armação do cliente: a montagem só vira preocupação depois de 7 dias aguardando a armação, e a persistência informa que o assunto continua sem resolução.
- Confirmado que “vendas de lentes sem OS” filtra apenas produtos `tipo_produto = Lente`; `LenteContato` fica fora. O texto da Central foi explicitado como lentes oftálmicas.
- Reativado o alerta de OS sem data prometida como ponto operacional relevante; ele volta a aparecer e ser enviado à IA quando houver registros nessa condição.
- Acrescentadas verificações de OS do fluxo óptico sem lente vinculada e OS com lente vinculada sem grau preenchido, sempre com os registros afetados como evidência. OS preliminares ou de lente de contato não entram automaticamente nessa inconsistência.
- A Central não alerta mais para mais de uma OS aberta na mesma venda: esse é um fluxo normal e não prova duplicidade ou erro.
- O versionamento foi separado em mudanças pendentes e deploys concluídos: `PENDING_RELEASE_CHANGES` acumula cada implementação sem mudar a versão; depois do deploy, essas mudanças formam uma nova entrada de `RELEASE_HISTORY`.
- Mudanças pendentes podem ser descartadas ou substituídas antes do deploy; nesses casos, `PENDING_RELEASE_CHANGES` deve ser corrigido para registrar apenas o código que permanece na entrega.
- O modal agora mostra três versões de deploy por vez e libera as anteriores ao rolar ou acionar o carregamento. O histórico não será mais descartado.
- O lote atual foi fechado como versão `1.02.03`; `PENDING_RELEASE_VERSION` e `PENDING_RELEASE_CHANGES` ficaram vazios. A primeira alteração do próximo lote deve abrir `1.02.04`, e alterações seguintes apenas completam essa mesma versão até o deploy.
- A auditoria da Central impediu que um snapshot pronto seja sobrescrito por uma atualização manual posterior. A consulta de vendas de lentes agora respeita também o limite final da data de referência.
- As validações de lente e grau da OS consultam `venda_itens` e só consideram vendas com produto `tipo_produto = Lente`; uma venda exclusiva de `LenteContato` não gera esse alerta.
- A IA continua responsável apenas pela narrativa: os cards permanecem factuais, e textos gerados que tragam número, explicação causal ou orientação sem evidência são descartados em favor do texto determinístico.
- A suíte padrão passou a executar os testes da Central, cobrindo imutabilidade de snapshot, exclusão de lente de contato e respostas adversariais da IA.

## Problemas encontrados ou pendências

- A análise confirmou que os quatro alertas removidos eram ruído operacional para a Loja 1 e que algumas situações decorrem de inconsistências de cadastro ou fluxo que serão corrigidas depois.
- O histórico local de migrações do Supabase ainda não está conciliado com o banco remoto; nenhuma migração foi reaplicada nesta etapa.
- Um relatório inexistente ainda não pode reconstruir fielmente o estado passado apenas com as tabelas operacionais atuais. O job noturno precisa gerar o primeiro snapshot no horário previsto; recuperar esse caso no futuro exigirá armazenar histórico dos estados de origem.

## Próximos passos

1. Validar visualmente a nova leitura operacional e a paginação do histórico de versões após o deploy. Consumo baixo.
2. Corrigir na origem os fluxos que permitem datas ausentes ou múltiplas OS quando isso for priorizado. Consumo médio.
3. Conciliar o histórico de migrações antes de qualquer `db push`. Consumo médio.

## Ideias futuras

- Manter a análise operacional concentrada em falhas reais do fluxo: lente chegada sem montagem local no prazo e espera prolongada pela armação do cliente.

# Diário - 25/08/2026

## O que foi feito

- O resumo financeiro da Central passou a contabilizar vendas pela `data_fechamento`, mantendo o filtro de status `Fechada`.
- Os acumulados do mês, comparativo anual, cobertura de custos e venda de lente sem OS passaram a usar a mesma data de fechamento.
- Os recebimentos continuam sendo apurados pela data efetiva de `pagamentos.data_pagamento`, separando venda realizada de dinheiro que entrou no caixa.
- A próxima versão foi aberta como `1.02.05` com essa alteração pendente, pois `1.02.04` já consta como deploy concluído.
- A atualização manual da Central força novo cálculo do dia solicitado e substitui o snapshot anterior apenas depois que o novo cálculo termina; o job automático continua preservando snapshots prontos.
- O relacionamento passou a analisar notas 1 ou 2 mesmo quando o pós-venda já foi concluído, além de reclamações, insatisfação e dificuldade de adaptação nos relatos. Esses sinais geram atenção; números normais de mensagens, respostas e pós-vendas concluídos não entram mais na leitura do módulo.
- Os sinais de relacionamento agora são classificados por data: ocorrência de ontem, acumulado do mês e histórico antigo. O alerta de tendência mensal só aparece ao atingir três sinais e o lifecycle impede a repetição diária de um caso já comunicado.
- A Central ganhou snapshots semanais e mensais imutáveis, consolidados a partir dos relatórios diários. Os botões de período apenas consultam o último snapshot salvo; o job cria a varredura no fechamento da semana ou do mês.
- O Radar Operacional passou a excluir OS de vendas canceladas ou devolvidas e OS cujo fluxo de laboratório já foi encerrado; o ajuste também cobre registros históricos, como a venda 124, sem alterar seus dados de auditoria.
- A tela da Central foi renomeada para “Pontos de Atenção”; o cabeçalho deixou de exibir “Central Diária”, os períodos ficaram discretos junto à referência e a atualização passou a ser representada apenas pelo ícone.
- O resumo financeiro passou a ocultar a comparação anual quando o histórico não existe, e os números das narrativas e alertas receberam destaque visual conforme a prioridade.
- Os cards de vendas e valores que entraram foram compactados, reduzindo aproximadamente 60% do espaço vertical entre as linhas.
- Os mesmos cards receberam uma segunda compactação visual, reduzindo novamente o padding das linhas e do contêiner sem alterar os dados.
- O título da seção de atenção passou a ser “Por favor dê atenção aos seguintes pontos:”, com orientação mais direta ao gerente.
- Os cards de atenção passaram a ocupar duas colunas em telas médias e grandes, mantendo uma coluna no celular.
- Os textos narrativos da IA passaram a usar mais largura e fonte maior, reduzindo quebras excessivas sem alterar os cards de evidência.
- O destaque das narrativas deixou de colorir números automaticamente: a IA agora pode marcar trechos críticos ou de atenção com marcadores semânticos, renderizados pelo frontend; os subtítulos dos módulos ganharam ícones Lucide.
- Os conteúdos de cada módulo passaram a ser expansíveis pelos subtítulos e começam fechados, deixando a página inicial mais limpa.
- Os controles de expansão receberam indicação visual explícita de abrir/fechar, seta mais visível e estado de hover para deixar a área clicável clara.
- Corrigida a renderização dos controles: o estado de expansão existia, mas o subtítulo ainda era um cabeçalho estático. Agora cada subtítulo renderiza um botão funcional com “Abrir/Fechar”.
- As cores das narrativas deixaram de ser escolhidas pela IA: ela relaciona o trecho a um `id` de alerta, e o frontend usa a prioridade auditável desse alerta para aplicar vermelho ou amarelo. O rótulo “Operação” também foi corrigido.
- O alerta de montagem passou a dizer que a data da montagem local ainda não foi preenchida, evitando a ideia incorreta de que existiria um registro separado de atraso.
- A Central ganhou o módulo diário “Cadastros”, voltado à faxina de uso do sistema: possíveis clientes duplicados por CPF, telefone ou nome normalizado; possíveis produtos duplicados por marca mais referência ou nome; produtos efetivamente vendidos nos últimos 90 dias sem custo positivo; e vendas ainda abertas há mais de sete dias.
- Cada suspeita preserva apenas os IDs dos registros afetados como evidência; a IA recebe fatos estruturados e gera a leitura amigável do módulo, enquanto regras auditáveis mantêm a prioridade e impedem conclusões quando uma fonte não pôde ser consultada.
- A tela passou a exibir “Cadastros” como quarto módulo expansível, com ícone próprio. O lifecycle diário evita repetir uma pendência estável sem mudança material.
- Quando ainda não houver snapshot salvo, a varredura semanal informa que a primeira leitura será disponibilizada na próxima segunda-feira e a mensal informa que será disponibilizada no primeiro dia do próximo mês.
- A operação passou a analisar a Gaveta pela data de montagem (`dt_montado_em`), e não pela chegada da lente. Óculos montados, sem entrega e parados por mais de sete dias geram atenção; acima de 30 dias são críticos por possível abandono ou baixa de entrega esquecida.
- A leitura também separa os casos sem telefone válido e os casos com telefone, mas sem aviso de retirada enviado pelo botão da Gaveta registrado em `whatsapp_outbound_messages`. Isso indica lacuna de registro, não prova que o cliente não foi avisado.
- Os cards operacionais com registros de OS agora exibem “Ver casos”. O modal protegido pelo mesmo PIN consulta somente as OS referenciadas no alerta e apresenta paciente, responsável, datas relevantes e link direto para a OS; os cards financeiros continuam sem ação nessa etapa.
- Alertas de relacionamento com registros de pós-venda agora abrem “Ver respostas” para revisão humana ou “Ver casos” nas demais situações. O modal mostra paciente, responsável, motivo, último resumo e data; o link abre a fila de Pós-venda com a OS correspondente selecionada quando ela continua na fila ativa.
- O módulo Cadastros ganhou filas protegidas pelo mesmo PIN em lotes de até dez: clientes e produtos suspeitos são apresentados lado a lado, produtos sem custo podem receber custo positivo no modal e vendas abertas antigas exibem os casos com link direto.
- Suspeitas sobrepostas passaram a formar um único grupo em cascata. Assim, registros ligados por evidências diferentes, como telefone e nome, não geram decisões concorrentes em grupos separados.
- O gerente pode marcar um grupo como registros distintos ou adiar a revisão por sete dias. As decisões são validadas novamente contra os registros atuais antes de serem aceitas; mesclagem de clientes e produtos permanece deliberadamente fora desta etapa.
- A migration `20260825160000_daily_health_data_quality_reviews.sql` criou a decisão atual e o histórico de eventos, registrando loja, registros afetados, estado anterior e posterior, usuário e funcionário gerente. Ela foi aplicada isoladamente e as duas tabelas foram confirmadas no banco remoto.
- A atualização de custo registra o valor anterior e o novo valor na auditoria. Ao fechar uma fila alterada, a Central recalcula uma única vez o snapshot para refletir os casos resolvidos.
- O agrupamento cadastral recebeu dois testes específicos; o typecheck e os 15 testes relacionados à Central passaram.

## Problemas encontrados ou pendências

- A conferência visual dos novos modais com dados reais ficou bloqueada porque o grant do PIN de gerente havia expirado no navegador; nenhuma decisão cadastral real foi executada durante a validação.
- O comando de lint não executa neste repositório porque a configuração atual do ESLint 9 falha ao serializar uma estrutura circular antes de analisar os arquivos.
- Mesclagem em cascata de clientes ou produtos ainda não existe. Ela exige inventário de dependências, transação atômica, prévia e estratégia de reversão antes de ser liberada.

## Próximos passos

1. Conferir no ambiente publicado o comparativo da Central com uma venda aberta em um dia e fechada em outro. Consumo baixo.
2. Conferir no ambiente publicado que a venda 124 não aparece mais no Radar. Consumo baixo.
3. Validar visualmente os quatro fluxos cadastrais com PIN de gerente e dados reais, sem confirmar decisões de duplicidade durante o primeiro smoke test. Consumo baixo.
4. Regenerar um snapshot da Loja 1 e conferir os alertas da Gaveta contra a tela operacional antes de adicionar ações assistidas de lembrete ou baixa retroativa. Consumo baixo.
5. Validar visualmente o modal de casos em um alerta operacional real da Loja 1 e decidir os primeiros botões de correção dentro da OS. Consumo baixo.
6. Validar visualmente o modal de respostas de pós-venda com casos reais e decidir as ações assistidas para cada motivo de contato pendente. Consumo baixo.
7. Mapear todas as referências de clientes e produtos e desenhar a prévia transacional da mesclagem antes de implementar qualquer alteração destrutiva. Consumo alto.

## Ideias futuras

- Adicionar teste automatizado de contrato das consultas financeiras para impedir que `created_at` volte a ser usado como data de venda.

- Criar teste de integração do Radar com uma venda cancelada que mantenha a OS histórica aberta.
- Evoluir a fila assistida para uma mesclagem em cascata com registro sobrevivente, prévia das referências movidas, transação atômica, idempotência e trilha de reversão.

# Diário - 26/08/2026

## O que foi feito

- Corrigido o critério de possíveis produtos duplicados da Central: nome, marca e referência agora são analisados em conjunto.
- A referência é comparada em formato compacto, portanto `RB 7195` e `RB7195` são equivalentes, mas `RB7195L` continua sendo outro produto.
- Nome e marca aceitam apenas equivalência normalizada ou erro simples de digitação, como `Ray Ban` e `Ray Bam`, sempre dentro da mesma referência; ausência de referência só combina com outra ausência.
- Atualizado o texto do alerta e do modal para explicar que a coincidência vem do conjunto dos três campos.
- Criados testes para referências diferentes, referência ausente versus informada e variações apenas de espaços ou de um caractere. Typecheck e 17 testes da Central passaram.
- Conferida a Loja 1 sem alterar dados: a nova regra encontrou 11 grupos e nenhum grupo envolveu produtos Ray Ban/Ray Bam.
- Implementado o passo 4 da deduplicação como prévia somente leitura. Em cada grupo, o gerente pode escolher qual cadastro seria o principal e consultar o impacto antes de qualquer mesclagem.
- O backend valida novamente o grupo e contabiliza as 24 relações de clientes e as seis relações de produtos existentes no banco, incluindo vendas, OS, financeiro, avaliações, WhatsApp, Torre, variantes e movimentos de estoque.
- A prévia mostra vínculos a transferir, dados que podem completar o cadastro principal, diferenças que exigem escolha e estoque resultante para produtos.
- CPF, RG, nascimento, referência, código de barras e tipo de produto divergentes são tratados como bloqueadores. Duas ou mais carteiras de crédito também bloqueiam a futura execução até existir uma regra de consolidação.
- A leitura real da Loja 1 foi validada sem exibir dados pessoais nem alterar registros: a primeira prévia de cliente encontrou dois vínculos e a primeira de produto encontrou três. Typecheck e 20 testes passaram.
- Implementado o passo 5 da deduplicação para os casos sem bloqueadores. Depois de escolher o cadastro principal na prévia, o gerente recebe uma segunda confirmação antes de executar a mesclagem.
- A API recalcula o grupo e a prévia imediatamente antes da execução, rejeitando alterações no lote ou novos conflitos. Uma chave de operação impede repetição inclusive em chamadas simultâneas.
- A função `merge_daily_health_duplicate_records` transfere em uma única transação todas as referências estrangeiras atuais de clientes ou produtos, completa apenas campos vazios do principal, soma o estoque dos produtos e remove os cadastros secundários somente ao final.
- A auditoria preserva os cadastros originais, as linhas dependentes transferidas, o destino, o gerente e a chave da operação. Qualquer erro provoca rollback integral; a função fica disponível somente ao `service_role`.
- A migration `20260826100000_daily_health_transactional_record_merge.sql` foi aplicada isoladamente. Assinatura, `security definer`, colunas de auditoria e bloqueio para perfis autenticados foram confirmados no banco remoto sem executar uma mesclagem real.
- Typecheck e os 22 testes específicos da Central passaram. A suíte geral ainda tem duas falhas preexistentes e alheias na Torre Electron, relacionadas ao contrato de rota e à URL empacotada.
- A escolha de cadastro principal ficou mais direta no modal: o botão de prévia agora aparece dentro de cada card com o rótulo "Prévia usando este como o principal", sem repetir o nome já visível. Os botões para adiar ou manter os registros separados continuam abaixo, pois decidem sobre o grupo inteiro.
- Cada card de cadastro suspeito agora exibe seu código, permitindo identificar diretamente o destino citado na prévia. O resumo de dependentes foi reescrito para explicar quantos vínculos serão transferidos e quantos já existem no cadastro principal.
- Implementado o passo 6 com recuperação assistida de mesclagens recentes. O gerente pode abrir o histórico no modal, identificar o cadastro principal e os incorporados e confirmar o desfazer.
- A função `undo_daily_health_record_merge` restaura os cadastros originais e devolve cada vínculo ao registro de origem em uma única transação. A recuperação é bloqueada se campos consolidados, vínculos transferidos ou IDs removidos mudaram depois da mesclagem.
- A auditoria da mesclagem passou a preservar também o estado final do cadastro principal. A migration `20260826120000_daily_health_merge_recovery.sql` foi aplicada isoladamente; as funções de mesclar e desfazer foram confirmadas como `security definer`, acessíveis somente pelo `service_role`.
- Typecheck, verificação de diff e os dez testes específicos de deduplicação e recuperação passaram. O banco permaneceu com zero eventos reais de mesclagem ou recuperação durante a implantação.
- O modal foi validado visualmente no localhost com PIN de gerente: histórico, estado vazio, códigos, coincidências e cards permaneceram alinhados. O indicador vermelho observado pertence a uma extensão do Chrome; não houve erro de console originado pelo localhost.
- A primeira mesclagem real de clientes foi executada, conferida diretamente no banco e desfeita pelo histórico. O cadastro secundário, os dados complementados e o vínculo de dependente foram restaurados corretamente; mesclagem e reversão permaneceram auditadas.
- A aba Mensal ganhou o botão “Sub-uso do programa”. O snapshot mensal passa a salvar uma análise determinística de funções desabilitadas, nunca utilizadas ou com queda forte contra a média dos três meses anteriores.
- O modal de sub-uso organiza os cards em Atendimento e vendas, Operação e laboratório, Relacionamento, Financeiro e fiscal e Estoque e catálogo. Funções sem configuração ou registros suficientes ficam silenciosas; a IA não participa dessa leitura.
- O analisador foi validado em modo somente leitura com a Loja 1 para julho: identificou queda de uso da cobrança e ausência histórica em contas a pagar e importação de XML. Typecheck, testes do classificador e verificação visual do estado inicial mensal passaram.
- O botão Refazer da aba Mensal agora permite gerar uma prévia do mês corrente usando os diários disponíveis até a última referência. A prévia inclui alertas e sub-uso do programa, fica apenas na tela e não é gravada como snapshot oficial; o fechamento automático mensal continua reservado ao fim do mês.
- Os textos fixos visíveis da Central de Pontos de Atenção e dos modais de operação, relacionamento e cadastros foram revisados para português brasileiro, incluindo tabelas, estados vazios, mensagens de erro, cards e ações de mesclagem. Identificadores e textos gerados pelo backend foram preservados.
- O acesso discreto abaixo da versão na tela inicial da loja agora se chama “Pontos de Atenção”. Ele abre a Central no resumo diário e solicita o PIN de gerente antes de exibir qualquer dado; o Sub-uso do programa permanece dentro da leitura mensal. Marca, versão e acesso têm brilho independente no hover, sem alterar os demais itens do rodapé.
- Quando um módulo diário não tiver mudança material, a Central passa a oferecer “Ver última atualização relevante”. O modal protegido pelo mesmo PIN percorre os snapshots diários salvos até encontrar o último alerta realmente exibido naquele módulo, em vez de mostrar apenas o relatório de ontem.
- O atalho “Pontos de Atenção” da tela inicial passou a pulsar de forma discreta até ser aberto uma vez no dia. O controle é local por loja e usa o dia de São Paulo; no dia seguinte o lembrete volta sem registrar dado operacional.
- O lifecycle de Cadastros deixou de silenciar pendências estáveis: grupos ainda existentes permanecem visíveis todos os dias até receberem mesclagem, decisão de manter separado ou adiamento. Financeiro, Operação e Relacionamento continuam usando a regra de não repetir casos sem mudança material.
- O cabeçalho de Pontos de Atenção passou a ter o botão de retorno à Central de Operações no padrão das páginas operacionais. A frase “Por favor dê atenção aos seguintes pontos:” agora usa a cor amarela de atenção.
- O cabeçalho de Pontos de Atenção passou a exibir “Pontos de Atenção (Beta)” e uma descrição da leitura noturna com a referência dinâmica do relatório; quando ainda não há relatório, mostra que a análise noturna não foi gerada.
- O módulo Pontos de Atenção deixou de ser restrito à Loja 1: a prévia mensal pode ser refeita em qualquer loja autorizada e o job noturno percorre todas as lojas cadastradas, gerando snapshots isolados por loja.
- O histórico pendente ganhou o destaque em caixa alta “IMPLEMENTAÇÃO DO MÓDULO PONTOS DE ATENÇÃO - A IA VASCULHA OS REGISTROS E AJUDA NA GESTÃO DA EMPRESA”.
- A versão `1.02.05` foi publicada. O histórico foi corrigido depois do deploy para mover todos os itens pendentes para `RELEASE_HISTORY`, limpar `PENDING_RELEASE_VERSION` e manter a próxima versão sem abertura até uma nova alteração.

## Problemas encontrados ou pendências

- A regra ainda depende da qualidade dos três campos cadastrados. Produtos sem referência só podem ser comparados com outros também sem referência, portanto podem continuar exigindo revisão humana quando o cadastro for incompleto.
- A Loja 1 ainda não possui snapshot mensal oficial salvo; a prévia manual pode ser gerada para teste antes do fechamento, enquanto o snapshot oficial continua aguardando o encerramento do período.

## Próximos passos

1. Usar o Refazer na aba Mensal da Loja 1 e conferir a prévia, os alertas e os cards de sub-uso. Consumo baixo.
2. Calibrar os limites de pouco uso depois de comparar os primeiros resultados mensais de lojas com perfis diferentes. Consumo médio.
3. Validar alguns grupos reais de produtos para calibrar se o limite de um caractere para nome e marca está conservador o suficiente. Consumo baixo.
4. Regenerar o snapshot diário da Loja 1 para confirmar que o grupo restaurado voltou à fila de duplicidades. Consumo baixo.
5. Avaliar casos bloqueadores individualmente antes de criar qualquer regra adicional de consolidação. Consumo alto.

## Ideias futuras

- Exibir no modal quais dos três campos coincidiram literalmente e qual foi tratado como provável erro de digitação, antes de oferecer uma decisão ao gerente.
- Criar dicas contextuais opcionais dentro de cada página para ensinar recursos pequenos, como copiar grau, busca universal e consulta de XML, sem misturá-los à leitura gerencial mensal.

# Diário - 27/08/2026

## O que foi feito

- A integração Pix Sicredi da branch `feature/integracao-pix-sicredi` foi migrada internamente de homologação para produção; não foi adicionada nenhuma escolha de ambiente à interface.
- O cliente Sicredi agora exige as variáveis `SICREDI_PIX_PROD_*`, valida o endpoint oficial de produção e utiliza as mesmas credenciais em criação, consulta, cancelamento, recuperação e baixa de cobranças de parcelas e do PDV Express.
- A liberação operacional permaneceu dupla: somente o CNPJ da Ótica Ocular e uma loja com `pix_provider = sicredi` podem acionar a integração.
- O prazo de cobrança do PDV Express passou a respeitar a mesma configuração de produção já usada nas parcelas.
- O teste de contrato, o typecheck e a autenticação OAuth mTLS em produção passaram. A autenticação retornou os escopos de cobrança e Pix; nenhuma cobrança de produção foi criada neste trabalho.
- A próxima versão pendente foi aberta como `1.02.06` com a migração da integração Sicredi para produção.
- O cliente Pix Sicredi passou a aceitar certificado, chave privada e cadeia em PEM codificados em Base64 para o ambiente hospedado, mantendo arquivos locais apenas como fallback de desenvolvimento. Nenhum segredo foi registrado no repositório.
- A baixa confirmada de uma parcela via consulta Sicredi agora encontra os pagamentos vinculados, abre o recibo automaticamente, atualiza o financeiro e fecha o modal Pix. A tabela de contas a receber mostra “Pix Sicredi” abaixo do valor recebido, sem afirmar que uma parcela parcialmente paga foi quitada.

## Problemas encontrados ou pendências

- Os três segredos Base64 dos PEMs ainda precisam ser configurados no ambiente de produção da Vercel e aplicados por um novo deploy; caminhos locais não funcionam na função hospedada.
- Ainda falta o primeiro teste financeiro controlado em produção com valor mínimo, em venda criada exclusivamente para esse fim.

## Próximos passos

1. Configurar os três segredos Base64 dos PEMs no ambiente Production da Vercel e fazer um novo deploy, sem versionar segredos. Consumo baixo.
2. Fazer um smoke test de produção com cobrança de valor mínimo e conta própria, confirmando QR Code, pagamento, webhook ou consulta, baixa automática e recibo. Consumo médio.
3. Revisar a tela de status depois do primeiro pagamento confirmado em produção. Consumo baixo.

## Ideias futuras

- Evoluir a configuração de credenciais para suportar provedores Pix por loja por meio de um cofre de segredos, sem expor uma credencial de uma loja a outra.

