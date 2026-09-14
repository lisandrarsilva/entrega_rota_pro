# Entrega Rota Pro 2.0

Aplicativo web estático para planejamento de rotas de entrega.

## Recursos

- Importa XLSX, XLSM, XLS e CSV.
- Detecta `Sequence`, `Destination Address`, `Bairro`, `City`, Latitude e Longitude automaticamente.
- Agrupa somente por rua equivalente + número + bairro + cidade quando essas colunas existem.
- Não usa similaridade difusa para o nome da rua. Isso evita juntar `Rua Mato Grosso` com `Rua Mato Grosso do Sul`.
- Reconhece abreviações seguras como `R`/`Rua`, `Av`/`Avenida`, `Rod.`/`Rodovia` e iniciais de palavras, por exemplo `R Edivaldo P da Silva` ↔ `Rua Edivaldo Pimenta da Silva`.
- Mantém todos os complementos: `Loja; Casa; Ap de frente o espetinho`.
- Mantém todas as colunas originais.
- Agrupa Sequence como `6, 7, 9`.
- Mapa com Leaflet/OpenStreetMap.
- Usa Latitude/Longitude da planilha quando disponíveis.
- Otimização local por proximidade (sem chave de API).
- Cálculo opcional de trajeto rodoviário via OSRM público.
- Reordenação manual por arrastar e soltar.
- Marcar entregue, problema e mandar para o final.
- Abrir parada no Google Maps.
- Pesquisa e filtros.
- Exportação `Grup_NomeOriginal.xlsx`.
- Tudo processado no navegador; a planilha não é enviada para um servidor próprio.

## Como publicar no Cloudflare

1. Extraia o ZIP.
2. Suba a pasta `public/` e o arquivo `wrangler.jsonc` para o GitHub.
3. No Cloudflare, conecte o repositório.
4. Build command: `None`.
5. Root directory: `/`.
6. Deploy command: `npx wrangler deploy`.
7. O `wrangler.jsonc` aponta os assets para `./public`, evitando publicar a pasta `.git` como conteúdo do site.
8. Publique.

## Observações sobre mapas e rotas

O mapa usa OpenStreetMap/Leaflet. O cálculo rodoviário usa o servidor público do OSRM. Para uso comercial ou alto volume, recomenda-se substituir por um provedor próprio/pago (ou backend com limites e cache).

## Base44

O arquivo `PROMPT_BASE44.md` contém um prompt longo para gerar uma versão full-stack no Base44, com banco, autenticação, histórico e sincronização.

## Teste recomendado

Com a planilha da Shopee:
- `6, 7, 9 / Avenida Amazonas, 661, Loja; Casa`
- `18, 21 / Rodovia Carmem Duarte, 575, Perto do ferro velho do lorim; Ap de frente o espetinho`
- `R Mato Grosso, 921` NÃO pode ser agrupado com `Rua Mato Grosso do Sul, 921`.
