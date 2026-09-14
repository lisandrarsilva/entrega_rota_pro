[PROMPT_BASE44.md](https://github.com/user-attachments/files/32214293/PROMPT_BASE44.md)
# Prompt para o Base44 — Entrega Rota Pro 2.0

Crie um aplicativo completo de logística de entregas chamado "Entrega Rota Pro 2.0", inspirado na experiência de planejadores de rotas como Spoke/Circuit, mas com foco em planilhas de entregas da Shopee e agrupamento inteligente de endereços.

## Objetivo

O usuário importa uma planilha de entregas e o sistema:
1. identifica automaticamente as colunas;
2. agrupa somente endereços que representam o mesmo local;
3. preserva todos os complementos;
4. preserva todas as colunas;
5. mostra as paradas no mapa;
6. permite otimizar a rota;
7. permite reordenar manualmente;
8. permite marcar entregas;
9. permite mandar uma parada para o final;
10. exporta uma nova planilha agrupada.

## Regra crítica de agrupamento

O agrupamento deve usar:
- mesma rua, aceitando abreviações seguras;
- mesmo número;
- mesmo bairro, se a coluna Bairro existir;
- mesma cidade, se a coluna City existir.

NÃO usar fuzzy matching agressivo.

Exemplo que DEVE agrupar:
- "Rua Edivaldo Pimenta da Silva, 671"
- "R Edivaldo P da Silva, 671"

Exemplo que NÃO DEVE agrupar:
- "Rua Mato Grosso, 921"
- "Rua Mato Grosso do Sul, 921"

A comparação deve ser baseada em tokens. Tipos de logradouro equivalentes: R/Rua, Av/Avenida, Rod./Rodovia, Tv/Travessa etc. Uma abreviação de uma letra pode corresponder ao início de uma palavra seguinte, como P → Pimenta, mas palavras extras não podem ser ignoradas.

## Complementos

O complemento NUNCA decide o agrupamento e NUNCA pode ser descartado.

Exemplo:
6 | Avenida Amazonas, 661
7 | Avenida Amazonas, 661, Loja
9 | Avenida Amazonas, 661, Casa

Resultado:
Sequence = "6, 7, 9"
Destination Address = "Avenida Amazonas, 661, Loja; Casa"

Outro:
18 | Rodovia Carmem Duarte, 575, Perto do ferro velho do lorim
21 | Rodovia Carmem Duarte, 575, Ap de frente o espetinho

Resultado:
Sequence = "18, 21"
Destination Address = "Rodovia Carmem Duarte, 575, Perto do ferro velho do lorim; Ap de frente o espetinho"

## Importação

Aceitar XLSX, XLSM, XLS e CSV.

Detectar automaticamente:
- Sequence
- Destination Address
- Bairro
- City
- Latitude
- Longitude
- SPX TN
- demais colunas

Não assumir ordem fixa das colunas.

## Exportação

Manter TODAS as colunas originais e a ordem original das colunas.

Para colunas agrupadas:
- Sequence: unir valores únicos em ordem numérica, separados por ", ".
- Destination Address: endereço base + complementos únicos separados por "; ".
- Latitude/Longitude: usar centróide médio dos registros do grupo.
- demais colunas: se iguais, manter um valor; se diferentes, unir valores únicos com " | ".

Nome:
Grup_NomeOriginal.xlsx

## Interface

Desktop e mobile.

Layout:
- barra superior com logo;
- painel esquerdo grande com mapa;
- painel direito com lista de paradas;
- indicadores: paradas, linhas/pacotes, entregues, distância;
- botões: Importar, Otimizar rota, Calcular trajeto, Restaurar ordem, Exportar;
- pesquisa;
- filtros Todas/Pendentes/Entregues/Problemas.

Cada parada deve mostrar:
- número da parada na rota;
- endereço;
- Sequence;
- quantidade de linhas agrupadas;
- bairro/cidade;
- status;
- botão Detalhes;
- botão Navegar;
- botão Entregue;
- botão Mandar para o final.

## Mapa

Usar mapa interativo.
Se Latitude/Longitude existirem, não geocodificar novamente.
Mostrar marcador numerado ou identificável para cada parada.
Mostrar marcador do início da rota.
Permitir enquadrar todas as paradas.
Desenhar a rota rodoviária quando possível.

## Otimização

Implementar otimização em duas camadas:
1. modo local sem API usando distância Haversine + nearest neighbor + melhoria 2-opt;
2. modo rodoviário opcional usando um provedor de rotas configurável.

Não obrigar o usuário a inserir uma API key para testar o aplicativo.

O início pode ser:
- localização atual do navegador;
- ponto escolhido;
- primeira parada.

## Operação durante a entrega

Status:
- Pendente
- Entregue
- Problema

Ação "Mandar para o final".

Arrastar e soltar para mudar a ordem manualmente.

Quando uma parada for marcada como entregue, atualizar os contadores.

## Histórico/aprendizado

Criar histórico local e, se o usuário estiver autenticado, histórico no banco.

Registrar:
- rota importada;
- ordem original;
- ordem final;
- alterações manuais;
- paradas entregues;
- problemas.

No futuro, usar esse histórico para sugerir preferências de rota, mas nunca alterar a rota automaticamente sem mostrar a sugestão.

## Banco e usuários

Criar entidades:
- Route
- Stop
- RouteEvent
- UserPreference

Permitir salvar uma rota para continuar depois.

## Navegação

Abrir Google Maps com destino da parada.
Preparar também links para Waze.

## Segurança

Planilhas devem ser processadas no navegador sempre que possível.
Não enviar dados de clientes para IA.
Não expor chaves de APIs no frontend; usar secrets/backend para integrações.

## Resultado

Quero um aplicativo funcional, não apenas uma landing page. Gere as páginas, componentes, entidades, workflows, estados e lógica necessários. Comece pela tela operacional da rota e pelo importador de Excel.
