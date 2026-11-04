# agroScoreMT

Ferramenta interativa de pré-análise de crédito rural com modelo logit, construída a partir do
Trabalho de Conclusão de Curso de **Gabrieli Saltareli** (Ciências Contábeis, UNEMAT, Campus de Sinop,
2026), *Modelo logit para estimação da probabilidade de aprovação de crédito rural em cooperativas de
Mato Grosso: desenvolvimento de ferramenta de pré-análise em planilha eletrônica*.
Orientador: Prof. Dr. Cleiton Franco.

Abra [`index.html`](index.html) no navegador. Não é preciso instalar nada.

## Abas da página

A página tem duas abas:

| Aba | Endereço | Conteúdo |
|---|---|---|
| Pré-análise | `#pre-analise` | Capa, ferramenta de pré-análise de um produtor (`#ferramenta`), análise da carteira de associados (`#carteira`) e o TCC (`#tcc`), em sequência |
| Relatório do imóvel | `#relatorio` | Leitura do PDF Farm-Scan e checagem socioambiental do imóvel |

Os dados aplicados em uma aba (área, impedimento, percentuais de soja e milho, ponto de corte) valem
na outra.

## O que a página contém

- **Ferramenta de pré-análise**: estima a probabilidade de inadimplência (PD) com os coeficientes do
  modelo principal e aplica a mesma regra de decisão da planilha `Ferramenta_CreditoRural_TCC.xlsx`
  (patrimônio imputado pela área, restrição no SFN como regra eliminatória, corte de Youden ou
  econômico com margem e LGD editáveis). Traz os exemplos e perfis do TCC prontos para carregar.
- **Receita por cultura**: a receita bruta é calculada pela área, separando soja (safra) e milho
  safrinha. Para cada cultura informam-se o percentual da área cultivada, a produtividade (sc/ha) e o
  preço (R$/sc). Os valores padrão são os do TCC (100% soja + 100% milho safrinha; 55,76 sc/ha a
  R$ 134,38 e 106,26 sc/ha a R$ 48,01), que reproduzem os resultados do trabalho. Outras composições
  aparecem com um aviso, porque o modelo foi estimado com a composição padrão.
- **Metodologia**: base de 2.867 imóveis de 20 municípios do norte de MT (área consolidada do CAR,
  Geoportal SEMA-MT, anonimizada), com classe de risco, endividamento e inadimplência simulados.
- **Resultados**: efeitos verdadeiros e estimados, Monte Carlo, comparação de modelos, efeitos
  marginais, curva ROC (AUC de 0,709 na amostra de teste) e calibração.
- **Ponto de corte**: explorador que recalcula a matriz de confusão dos 860 produtores de teste.

## Relatório do imóvel (Farm-Scan / Dados Fazenda)

Na aba **Relatório do imóvel** (`#relatorio`), carregue o
PDF do *Relatório da Propriedade* gerado em dadosfazenda.com.br. A aba mostra também o resultado atual
da ferramenta, e o selo vermelho na aba indica quantos impedimentos o relatório carregado tem. A página lê o PDF no próprio navegador (pdf.js) e:

- extrai a **área de uso consolidado do CAR**, que é a variável de área do modelo, além da área total,
  dos módulos fiscais, da vegetação nativa e do uso do solo;
- monta uma checagem por grupo (cadastro, ambiental, sobreposições, produtivo e clima), com níveis
  *impedimento*, *alerta*, *sem pendência* e *informativo*. São **impedimentos**: CAR suspenso ou
  cancelado, embargo IBAMA/ICMBio, desmatamento no PRODES e sobreposição com terra indígena, unidade
  de conservação ou território quilombola. São **alertas**, entre outros: queimadas recentes, passivo
  de Reserva Legal, processos minerários sobre a maior parte do imóvel, ausência de lavoura no uso do
  solo, safra mais recente antiga na CONAB, aptidão agrícola inapta e recorrência de seca;
- leva os dados para a ferramenta: aplica a área consolidada e ativa a nova regra eliminatória
  **impedimento socioambiental** (com prioridade sobre a PD, como a restrição no SFN). O percentual
  de soja e milho pela lavoura detectada pode ser aplicado com um clique.

A checagem é uma triagem automática. Cada ponto deve ser confirmado nas bases oficiais (SICAR, IBAMA,
INPE, ANM) antes do parecer.

## Rotina para cooperativas: análise da carteira

Na aba **Carteira** (`#carteira`), a cooperativa analisa todos os associados de uma vez:

1. Baixe o modelo ([`modelo_associados.csv`](modelo_associados.csv), também disponível pelo botão na
   página) e preencha uma linha por associado. Obrigatórias: `area_ha`, `endividamento_sfn` e
   `classe` (1 a 10 ou R1 a R10). Opcionais: `codigo`, `nome`, `municipio`, `valor_solicitado`,
   `patrimonio_terras`, `restricao_sfn`, `pct_soja`, `pct_milho` e `impedimento_socioambiental`.
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
