# 🏅 Olympic Medal Rankings

Análise histórica do quadro de medalhas olímpicas (1896–2024), separando Jogos de Verão e de Inverno e combinando ambos em um ranking geral.

## Sobre

O notebook processa dados de medalhas de 1896 a 2022 (`medals_1896-2022.csv`) mais os resultados de Paris 2024 (`medals_2024.csv`), constrói rankings por país e gera visualizações com os 50 países mais bem colocados em cada categoria.

## Estrutura

```
.
├── tables/
│   ├── medals_1896-2022.csv
│   └── medals_2024.csv
├── main.ipynb
└── README.md
```

## Pré-requisitos

- [uv](https://docs.astral.sh/uv/) instalado

## Instalação e execução

### Com uv (recomendado)

```bash
git clone <repo-url>
cd olympic-medals

uv sync
uv run jupyter lab main.ipynb
```

### Com pip

```bash
git clone <repo-url>
cd olympic-medals

python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate

pip install polars matplotlib numpy jupyter
jupyter lab main.ipynb
```

## Dependências

Gerenciadas pelo `uv` via `pyproject.toml`:

- **polars** — manipulação dos dados
- **matplotlib** — geração dos gráficos
- **numpy** — operações auxiliares nos plots
- **jupyter** — execução do notebook

## Pipeline

1. Leitura dos CSVs e filtragem de linhas sem medalha
2. Deduplicação por `(edition_id, event, country_noc, medal)` para evitar dupla contagem em esportes coletivos
3. Pivot para colunas `Gold`, `Silver`, `Bronze` separadas por temporada (Summer / Winter)
4. Soma com os dados de Paris 2024
5. Join entre Summer e Winter para montar `df_full` com colunas nomeadas (`Gold_Summer`, `Gold_Winter`, etc.)
6. Criação dos rankings:
   - `df_summer_rank` — ordenado por `Total_Summer`
   - `df_winter_rank` — ordenado por `Total_Winter`
   - `df_overall_rank` — soma das duas temporadas, ordenado por `Total_Geral`
7. Gráficos de barras horizontais empilhadas (Gold / Silver / Bronze) para os top 50 de cada ranking

**README feito por IA**