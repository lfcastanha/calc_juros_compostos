# Calculadora de Juros Compostos

Site estático (um único `index.html`) que simula investimentos com juros compostos, baseado na planilha
`calc_juros_compostos-2026.xlsx` (abas *inicial*, *tabela* e *Projecao_salarial*).

## Como usar

Abra o `index.html` no navegador ou acesse a versão publicada:
**https://lfcastanha.github.io/calc_juros_compostos/**

### Publicação (GitHub Pages)

O workflow `.github/workflows/pages.yml` publica o site automaticamente a cada push no branch `main`
(também pode ser disparado manualmente em *Actions → Publicar no GitHub Pages → Run workflow*).

Configuração única no repositório:
1. *Settings → Pages → Build and deployment → Source*: **GitHub Actions**.
2. *Settings → General → Default branch*: **main**.
O gráfico usa Chart.js via CDN; sem internet, os resultados e a tabela continuam funcionando.

## Funcionalidades

- Valor inicial, aporte mensal e **aumento anual do aporte** (aporte do ano *n* = aporte inicial + (n − 1) × aumento)
- Taxa de juros anual ou mensal; período em anos ou meses (até 100 anos)
- **Opções avançadas** (recolhidas por padrão): começar a aportar só após X anos/meses, parar de aportar após
  X anos/meses (o patrimônio continua rendendo fora dessa janela), inflação e imposto de renda
- Resultados: valor total final, total investido, total em juros, valor líquido após IR,
  valor líquido real (descontada a inflação) e renda mensal passiva estimada
- Gráfico de barras (composição investido × juros, ou aporte × juros do mês)
- Tabela mensal ou anual, com exportação para CSV
- Cálculo do aporte a partir do salário (salário, aumento salarial anual e % investido), como na aba *Projecao_salarial*
- **Cenários**: salve simulações com nomes (aportes, taxas e prazos diferentes), reabra no simulador, atualize,
  renomeie ou exclua. Os cenários ficam guardados no navegador (localStorage) e podem ser exportados/importados
  em arquivo `.json` para backup ou para usar em outro computador.
- **Comparação de cenários** (marque quais entram): gráfico de linhas com a evolução de cada cenário
  (total acumulado, líquido real, investido ou juros), gráfico de barras com o resultado final
  (investido × juros) e tabela lado a lado com parâmetros e resultados, destacando o melhor valor e exportável em CSV
- Tema claro/escuro e layout responsivo

## Lógica de cálculo (igual à planilha)

- Taxa mensal equivalente: `(1 + taxa_anual)^(1/12) − 1`
- Mês 0: o valor inicial rende um mês de juros
- O aumento anual do aporte conta a partir do primeiro aporte mensal (relevante quando os aportes começam depois)
- Mês *m*: o aporte entra no início do mês e rende no mesmo mês:
  `juros_m = (total_investido_m + juros_acumulados_{m−1}) × taxa_mensal`

Com os valores padrão da planilha (R$ 0 inicial, R$ 1.000/mês, +R$ 500/ano, 13% a.a., 12 anos) o resultado é
**R$ 1.002.615,06**, idêntico ao da planilha.

### Diferenças em relação à planilha

- Na planilha, a coluna "REAL" aplica o IR sobre o valor total (inclusive o que foi investido). No site, o IR incide
  **apenas sobre os juros**, como acontece de fato no Brasil.
- A inflação, que estava na planilha mas não era usada, agora gera o "valor líquido real" (em dinheiro de hoje).
