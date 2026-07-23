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
