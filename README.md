[README.md](https://github.com/user-attachments/files/32214296/README.md)
# Entrega Rota Pro 6.0

Aplicativo web para planejamento e execução de rotas de entrega.

## Principais recursos
- Importação XLSX, XLSM, XLS e CSV.
- Usa Latitude/Longitude da planilha para posicionar as paradas; endereço, bairro e cidade continuam disponíveis para identificação e agrupamento.
- Agrupamento seguro por rua + número + bairro/cidade, preservando Sequence e complementos.
- Não usa fuzzy matching agressivo: Rua Mato Grosso não é agrupada com Rua Mato Grosso do Sul.
- Mostra quantidade de pacotes por parada.
- Na lista/detalhes, exibe apenas Sequence, Destination (quando existir), Address/Destination Address, Bairro e City, cada informação em sua própria linha.
- Edição da ordem clicando nos balões do mapa.
- Edição manual da localização: arraste o balão, acompanhe a posição durante o movimento e, ao soltar, o sistema consulta o endereço do ponto, permite editar/confirmar e salva a nova latitude/longitude.
- Status: Entregue, Pendente, Não entregue e Problema.
- Fim: envia a parada para o final da rota.
- Ponto de partida pela localização atual e ponto final pela casa.
- Otimização local e cálculo de trajeto via OSRM público.
- Navegação pelo Google Maps.
- Exportação preservando todas as colunas da planilha.

### Observação sobre a confirmação de endereço
A confirmação automática após arrastar o balão usa um serviço de geocodificação reversa público (OpenStreetMap/Nominatim), porque a API de geocodificação do Google exige uma chave/API configurada. A posição salva é exatamente a coordenada escolhida no mapa, e o endereço retornado pode ser editado antes da confirmação.
