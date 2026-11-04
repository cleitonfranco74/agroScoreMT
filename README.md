# AgroScoreMT

Ferramenta interativa de pré-análise de crédito rural com modelo logit, construída a partir do
Trabalho de Conclusão de Curso de **Gabrieli Saltareli** (Ciências Contábeis, UNEMAT, Campus de Sinop,
2026), *Modelo logit para estimação da probabilidade de aprovação de crédito rural em cooperativas de
Mato Grosso: desenvolvimento de ferramenta de pré-análise em planilha eletrônica*.
Orientador: Prof. Dr. Cleiton Franco.

Abra [`index.html`](index.html) no navegador. Não é preciso instalar nada.

## Abas da página

A página tem três abas:

| Aba | Endereço | Conteúdo |
|---|---|---|
| Pré-análise | `#pre-analise` | Capa, ferramenta de pré-análise de um produtor (`#ferramenta`), análise da carteira de associados (`#carteira`) e o TCC (`#tcc`), em sequência |
| Relatório do imóvel | `#relatorio` | Consulta pública do imóvel pelo código do CAR: limite no SICAR e desmatamento no PRODES/INPE |
| Súmula de risco | `#sumula` | Leitura do PDF da súmula de análise de risco do cooperado |

Os dados aplicados em uma aba (área, impedimento, percentuais de soja e milho, ponto de corte) valem
na outra.

## O que a página contém

- **Ferramenta de pré-análise**: estima a probabilidade de inadimplência (PD) com os coeficientes do
  modelo principal e aplica a mesma regra de decisão da planilha `Ferramenta_CreditoRural_TCC.xlsx`
  (patrimônio imputado pela área, restrição no SFN como regra eliminatória, corte de Youden ou
  econômico com margem e LGD editáveis). Traz os exemplos e perfis do TCC prontos para carregar.
- **Receita por atividade (formato híbrido)**: a receita bruta é calculada pela área, somando soja
  (safra), milho safrinha, algodão (1ª ou 2ª safra) e pecuária de corte (boi). Para cada atividade
  informam-se o percentual da área e os parâmetros: soja e milho em sc/ha e R$/sc (padrão do TCC:
  55,76 sc/ha a R$ 134,38 e 106,26 sc/ha a R$ 48,01); algodão em @ de pluma/ha, R$/@ e custo por
  hectare; boi em @/ha/ano, R$/@ e custo por hectare ao ano. Algodão e boi começam em 0% e trazem
  valores de referência para simulação (120 @/ha a R$ 130/@ e custo de R$ 13.500/ha; 7 @/ha/ano a
  R$ 300/@ e custo de R$ 1.400/ha), que devem ser atualizados com os dados do IMEA da safra. A página
  avisa quando a soja, o algodão de 1ª safra e a pastagem passam de 100% da área, ou quando a 2ª safra
  ocupa mais que a área de soja. Como o modelo foi estimado com 100% soja + 100% milho safrinha, outras
  composições são lidas como cenário.
- **Metodologia**: base de 2.867 imóveis de 20 municípios do norte de MT (área consolidada do CAR,
  Geoportal SEMA-MT, anonimizada), com classe de risco, endividamento e inadimplência simulados.
- **Resultados**: efeitos verdadeiros e estimados, Monte Carlo, comparação de modelos, efeitos
  marginais, curva ROC (AUC de 0,709 na amostra de teste) e calibração.
- **Ponto de corte**: explorador que recalcula a matriz de confusão dos 860 produtores de teste.

## Relatório do imóvel: consulta pública SICAR + PRODES

Na aba **Relatório do imóvel**, digite o código do CAR e clique em *Consultar*. A página, direto do
navegador:

1. busca o limite do imóvel no GeoServer público do SICAR (`geoserver.car.gov.br`, camada
   `sicar:sicar_imoveis_<uf>`, filtro `cod_imovel`), com situação, condição, área declarada e módulos
   fiscais;
2. busca os polígonos de desmatamento anual do PRODES no TerraBrasilis/INPE
   (`prodes-legal-amz:yearly_deforestation` e `prodes-cerrado-nb:yearly_deforestation`) na área do
   imóvel e calcula, com Turf.js, os hectares desmatados **dentro** do perímetro em cada ano;
3. procura a **área de uso consolidado**, que é a variável de área do modelo e a base da receita: lê a
   lista de camadas do GeoServer do SICAR (`GetCapabilities`), escolhe a camada de área consolidada da
   UF e soma a parte que cai dentro do perímetro. Se encontrar, preenche o campo *Área de uso
   consolidado (ha)* e aplica à ferramenta; se não, o analista informa o valor do demonstrativo do CAR.
   O campo mostra na hora a receita projetada (soja e milho safrinha), o resultado operacional, a
   alavancagem e a parcela do perímetro;
4. mostra o mapa, a tabela por ano e a checagem: CAR suspenso ou cancelado e desmatamento após
   22/07/2008 são **impedimentos** (aplicáveis à ferramenta como impedimento socioambiental); o
   desmatamento anterior a 2008 é informativo.

Se o SICAR não responder ou recusar a consulta do navegador, carregue o perímetro do imóvel baixado da
consulta pública do CAR (`.zip` com shapefile, `.kml` ou `.geojson`); o cruzamento com o PRODES é feito
do mesmo jeito. O PRODES é anual e tem área mínima de mapeamento; não substitui DETER, MapBiomas Alerta
nem vistoria.

## Súmula de risco do cooperado

Na aba **Súmula de risco** (`#sumula`), carregue o PDF da *Súmula de análise de risco* emitida pelo
sistema da cooperativa. A página lê o PDF no navegador e:

- extrai o **endividamento no SFN** (base atual), o **risco CRL** (usado como classe R1–R10 do modelo)
  e a **anotação Serasa/Bacen** (aplicada como restrição no SFN, regra eliminatória);
- mostra os oito indicadores ponderados (base anterior, base atual, variação e nota), a classificação
  e a pontuação da súmula, os fatores adversos, os fundamentos favoráveis e as providências recomendadas;
- compara a alavancagem pela renda cadastral com a alavancagem pela receita estimada pela área e avisa
  quando a área da ferramenta parece incompatível com a renda do cooperado.

**Posição no SFN.** A aba mostra o painel *Endividamento do cooperado no SFN* (SCR): dívida total e
variação em 12 meses, parcela que vence em até 360 dias e longo prazo, parte na cooperativa e em outras
instituições, saldo vencido, e as relações dívida ÷ renda, curto prazo ÷ renda (comprometimento), dívida
÷ receita estimada pela área (alavancagem do modelo) e saldo em conta ÷ curto prazo, com alertas
(crescimento forte, concentração no curto prazo, comprometimento acima de 50%, vencidos, liquidez baixa,
pouca exposição na cooperativa e área incompatível com a renda). Quando a súmula é aplicada, a
ferramenta exibe esse resumo do SFN junto ao resultado.

A súmula contém dados pessoais e financeiros sigilosos: o PDF não sai do navegador e o CPF aparece
mascarado. O exemplo exibido ao abrir a aba é fictício.

## Rotina para cooperativas: análise da carteira

Na aba **Carteira** (`#carteira`), a cooperativa analisa todos os associados de uma vez:

1. Baixe o modelo ([`modelo_associados.csv`](modelo_associados.csv), também disponível pelo botão na
   página) e preencha uma linha por associado. Obrigatórias: `area_ha`, `endividamento_sfn` e
   `classe` (1 a 10 ou R1 a R10). Opcionais: `codigo`, `nome`, `municipio`, `valor_solicitado`,
   `patrimonio_terras`, `restricao_sfn`, `pct_soja`, `pct_milho`, `impedimento_socioambiental`,
   `endividamento_curto_prazo` (parte da dívida no SFN que vence em até 360 dias), `renda_anual`,
   `pct_algodao` e `pct_pastagem`. Com
   as duas últimas, a carteira mostra dívida ÷ renda e curto prazo ÷ renda e aponta comprometimento acima
   de 50% e área incompatível com a renda.
2. Na ferramenta, defina o ponto de corte, a margem, a LGD e os parâmetros de soja e milho.
3. Carregue o arquivo (CSV separado por ponto e vírgula ou vírgula, ou Excel .xlsx). Aceita números no
   formato brasileiro (1.234.567,89) e nomes de colunas com acentos ou maiúsculas.
4. Veja o resumo (aprováveis, não aprováveis, restrições, impedimentos, PD média e perda esperada dos
   aprováveis), filtre, ordene e abra qualquer associado na ferramenta.
5. Exporte os resultados em CSV ou copie a tabela para o Excel.

Os arquivos são processados apenas no navegador e não são enviados a nenhum servidor. Mesmo assim,
prefira códigos internos a nomes e CPF (LGPD).

O modelo usa PD = 1 / (1 + e^−XB), com
XB = −1,3870 + 0,1469·Classe + 1,0832·ln(Alavancagem) − 0,1594·ln(Área).

> Uso didático. A classe de risco, o endividamento e a inadimplência foram simulados; os coeficientes
> não representam o risco de produtores reais nem a política de nenhuma instituição.

## Publicar como site

Em *Settings → Pages*, escolha *Deploy from a branch*, a branch com a página e a pasta `/ (root)`.
O site fica em `https://cleitonfranco74.github.io/agroScoreMT/`.
