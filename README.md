# agroScoreMT

Ferramenta interativa de pré-análise de crédito rural com modelo logit, construída a partir do
Trabalho de Conclusão de Curso de **Gabrieli Saltareli** (Ciências Contábeis, UNEMAT, Campus de Sinop,
2026), *Modelo logit para estimação da probabilidade de aprovação de crédito rural em cooperativas de
Mato Grosso: desenvolvimento de ferramenta de pré-análise em planilha eletrônica*.
Orientador: Prof. Dr. Cleiton Franco.

Abra [`index.html`](index.html) no navegador. Não é preciso instalar nada.

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

O modelo usa PD = 1 / (1 + e^−XB), com
XB = −1,3870 + 0,1469·Classe + 1,0832·ln(Alavancagem) − 0,1594·ln(Área).

> Uso didático. A classe de risco, o endividamento e a inadimplência foram simulados; os coeficientes
> não representam o risco de produtores reais nem a política de nenhuma instituição.

## Publicar como site

Em *Settings → Pages*, escolha *Deploy from a branch*, a branch com a página e a pasta `/ (root)`.
O site fica em `https://cleitonfranco74.github.io/agroScoreMT/`.
