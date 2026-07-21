# Diário - 20/07/2026

## O que foi feito

- Reorganizado o menu inicial da Torre: “Continuar atendimento” passou a usar o mesmo menu de experiências de “Novo atendimento”, com seleção das seções abertas por data, hora e cliente.
- Corrigidos avisos de uso síncrono de `cookies()` nas rotas do Next/Supabase.
- Reforçada a navegação do Electron para impedir que o touch caia no login ou em rotas administrativas.
- Melhorado o fluxo do PIN administrativo, exibindo a troca de PIN somente quando solicitada.
- Implementada a persistência das aprovações de câmera, touch e tela do cliente no SQLite local, com outbox para sincronização no Supabase.
- Criada e aplicada a migration de aprovações de hardware no Supabase, vinculada ao ativo físico da Torre.
- A faixa de status da Torre deixou de usar “Tela cliente conectada” e “Câmera pronta” fixos; agora consulta o estado real do Electron e das aprovações.
- Removidos da tela do cliente os alertas de medidas que poderiam prejudicar a venda. Esses alertas continuam disponíveis para o funcionário e são gravados no resultado técnico.

## Problemas encontrados

- As imagens frontal e de perfil da tela de Medidas ainda ficam somente na memória da página. Ao sair e retornar à mesma seção, os resultados numéricos permanecem no fluxo, mas as imagens desaparecem.
- O Electron precisa ser reiniciado após alterações no `preload.cjs`; atualizar apenas a página não carrega novas funções IPC.
- A tela de Medidas ainda precisa de um rascunho local para recuperar imagens e estado parcial da seção.

## Próximos passos

1. Persistir no SQLite os rascunhos de Medidas vinculados ao `tower_session_id`.
2. Recuperar automaticamente imagens frontal e de perfil ao retornar à seção ativa.
3. Definir a política de descarte das imagens ao concluir ou descartar o atendimento.
4. Validar o fluxo completo no Electron com duas telas, câmera e touch reais.

## Ideias futuras

- Manter fotos de Medidas somente no equipamento, protegidas e fora do Supabase por padrão.
- Exibir no funcionário um histórico resumido das revisões e correções feitas nas medidas.
- Criar um estado visual de sincronização da Torre no menu principal.

## Ideia futura � arma��es 3D para espessura

A tentativa inicial com `armacao.dae` n�o produziu resultado aceit�vel porque usou a malha original dos vidros como base. Para uma vers�o futura, exportar do SketchUp a arma��o sem lentes renderizadas, com grupos ou contornos-guia nomeados `encaixe_OD` e `encaixe_OE`, al�m de refer�ncias de plano frontal, eixo de profundidade e centro �ptico. Esses contornos ser�o usados como estrutura para reconstruir no Three.js uma lente s�lida com frente curva, traseira recuada pela espessura calculada e parede lateral fechada. A ideia permanece futura e exige calibra��o geom�trica expl�cita.

# Diario - 21/07/2026

## O que foi feito

- Corrigido o fluxo de pedido de chave Pix no WhatsApp: a chave e enviada e a conversa entra em pausa para atendimento humano.
- Corrigido o radar operacional do WhatsApp para atualizar o contador de pendencias automaticamente a cada 30 segundos, sem depender da abertura da central operacional.
- Ajustados os campos de cilindro das OSs na Vendas Experimental para assumirem o sinal negativo ao receber foco; por exemplo, 025 passa a -0,25.

## Problemas encontrados ou pendencias

- O aviso de uso sincronico de cookies no dashboard da loja continua pendente de migracao da fabrica legada createClient() para createAsyncClient().
- A validacao visual do radar e dos campos de grau ainda precisa ser feita no ambiente com mensagens reais e uma OS aberta.

## Proximos passos

1. Validar o radar com uma nova mensagem que gere handoff humano.
2. Testar os campos de esferico e cilindro em todas as paginas filhas de OS.
3. Migrar os usos restantes de createClient() sincronico no servidor.

## Ideias futuras

- Substituir o refresh periodico do radar por atualizacao em tempo real via evento ou polling dedicado de menor custo.
