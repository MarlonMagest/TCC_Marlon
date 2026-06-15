# TCC — Marlon Patrick Magest Filgueiras

**Perfil Físico dos Elencos como Fator de Vitória no Futebol Europeu: Uma Análise Comparativa entre Ligas**

Trabalho de Conclusão de Curso apresentado ao Centro de Ciências Sociais Aplicadas da Universidade Católica de Petrópolis (UCP), como requisito parcial para obtenção do grau de Bacharel em Ciência de Dados e Business Intelligence. **Orientador:** Prof. Dr. Rodolfo Nicolay

---

## Pergunta central

> O perfil físico dos elencos — medido pelas diferenças médias de altura, peso e idade entre mandante e visitante — é fator associado à probabilidade de vitória no futebol europeu? Esse efeito varia entre ligas?

---

## Dados

- **Base:** European Soccer Database — [download aqui](https://www.kaggle.com/datasets/hugomathien/soccer/data)
- **Formato:** SQLite (`database.sqlite`)
- **Tabelas utilizadas:** `Match`, `Player`, `League`, `Country`
- **Escopo:** 11 ligas europeias | temporadas 2008/09 a 2015/16 | 24.185 partidas
- **Excluído por design:** `Player_Attributes` (ratings subjetivos do FIFA — apenas no Apêndice)

---

## Pipeline do notebook

| Etapa | Conteúdo |
|-------|----------|
| 0 | Dependências e configuração |
| 1 | Extração SQL, JOIN e engenharia de atributos |
| 2 | Análise exploratória (6 figuras) |
| 3 | Testes estatísticos formais |
| 4 | Modelagem supervisionada |
| Apêndice | Comparação: modelo físico vs. modelo com atributos FIFA |

---

## Variáveis criadas

| Variável | Descrição |
|----------|-----------|
| `diff_height` | Altura média mandante − visitante (cm) |
| `diff_weight` | Peso médio mandante − visitante (kg) |
| `diff_age` | Idade média mandante − visitante (anos) |
| `home_win` | 1 = vitória do mandante, 0 = empate ou derrota |

---

## Modelagem

- Regressão Logística (principal) e Random Forest, ambos com `class_weight='balanced'` para corrigir o leve desbalanceamento das classes (45,7% de vitórias vs. 54,3% de não vitórias)
- Validação cruzada estratificada de 5 dobras (`StratifiedKFold`, `random_state=42`)
- Métricas: ROC-AUC (principal), Acurácia, F1-Score e Brier Score (calibração)
- Multicolinearidade verificada por VIF (todos abaixo de 2)

---

## Principais resultados

- `diff_weight` e `diff_age` apresentaram associação significativa com `home_win` (p < 0,001)
- ROC-AUC global: **≈ 0,532** (Regressão Logística) — superior ao acaso, efeito fraco
- Efeito varia expressivamente entre ligas:
  - **Jupiler League e Premier League:** as três variáveis são significativas (AUC até 0,585)
  - **La Liga:** nenhuma variável significativa
  - **Bundesliga e Eredivisie:** apenas `diff_age` significativa

---

## Como executar

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn
jupyter notebook TCC_Notebook_Marlon_Final.ipynb
```

Faça o download do arquivo `database.sqlite` em [kaggle.com/datasets/hugomathien/soccer/data](https://www.kaggle.com/datasets/hugomathien/soccer/data) e coloque-o na mesma pasta do notebook antes de executar.

---

## Tecnologias

`Python` · `pandas` · `NumPy` · `scikit-learn` · `SciPy` · `Matplotlib` · `Seaborn` · `SQLite`

---

## Nota sobre uso de IA

Ferramentas de inteligência artificial foram utilizadas para correção ortográfica e gramatical do texto e para o aprimoramento visual dos gráficos. Todo o conteúdo analítico, as decisões metodológicas e a interpretação dos resultados são de inteira responsabilidade do autor.

---
