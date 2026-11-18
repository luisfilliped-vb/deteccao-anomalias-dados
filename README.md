# Detecção automática de anomalias em dados

Trabalho da disciplina de Banco de Dados da UNIPAMPA, grupo 7. Compara regras tradicionais, Isolation Forest e Local Outlier Factor (LOF) em dados sintéticos de vendas.

## Arquivos

- `trabalho_bd.py`: geração dos dados, experimentos, dez repetições de tempo, cenário complementar e matriz de confusão.
- `requirements.txt`: bibliotecas necessárias.
- `.gitignore`: exclui ambientes locais e arquivos gerados a cada execução.

## Como executar

Com Python 3 instalado, abra um terminal na pasta do projeto:

```bash
python -m venv .venv
```

Ative o ambiente no Windows (PowerShell):

```powershell
.\.venv\Scripts\Activate.ps1
```

Ou no Linux/macOS:

```bash
source .venv/bin/activate
```

Instale as dependências e execute:

```bash
python -m pip install -r requirements.txt
python trabalho_bd.py
```

No Google Colab, envie `trabalho_bd.py` e execute uma célula com `%run trabalho_bd.py`.

O programa imprime as métricas e gera, na pasta de execução:

- `vendas_experimento.csv`: base original rotulada;
- `tempos_individuais.csv`: 30 medições, dez por método;
- `resumo_tempos.csv`: média, desvio padrão amostral e mediana;
- `matriz_confusao_lof.png`: matriz do LOF no cenário complementar.

## Procedimento experimental

A base contém 1.000 registros: 950 normais e 50 com alterações inseridas. A geração utiliza semente 42. No cenário original são inseridos 20 preços de -30, 15 preços de 50.000 e 15 quantidades de 1.000.

As regras sinalizam preço menor que zero, preço maior que 10.000 ou quantidade maior que 100. Os modelos utilizam preço relativo ao preço de referência do produto e quantidade, padronizados com StandardScaler. Os rótulos são usados para avaliação, não como atributos de entrada.

| Modelo | Configuração |
|---|---|
| Isolation Forest | `n_estimators=100`, `contamination=0.05`, `random_state=42` |
| LOF | `n_neighbors=20`, `contamination=0.05`; distância padrão Minkowski com `p=2` |

No cenário complementar, a semente é 123. Os 20 preços antes negativos passam a 18%–30% acima da referência; os 15 preços extremos passam a 25%–40% acima da referência; as 15 quantidades extremas passam a valores inteiros de 15 a 30. Permanecem os mesmos 50 rótulos de anomalia. A padronização é refeita nesse cenário.

As dez repetições medem apenas a aplicação das regras ou `fit_predict` dos modelos no cenário original. Preparação dos atributos, construção dos modelos e cálculo de métricas ficam fora dessa medição. O script imprime métricas individuais de regras e Isolation Forest no cenário original e de todos os três métodos no cenário complementar; o LOF também participa das dez medições originais.

## Resultados anteriormente observados no Colab

Os valores abaixo vêm da execução anterior documentada pelo autor, e não de uma execução desta cópia local.

| Método no cenário complementar | Alertas | Precisão | Recall | F1 |
|---|---:|---:|---:|---:|
| Regras tradicionais | 0 | 0 | 0 | 0 |
| Isolation Forest | 50 | 1,00 | 1,00 | 1,00 |
| LOF | 50 | 0,46 | 0,46 | 0,46 |

No LOF foram 23 verdadeiros positivos, 27 falsos positivos, 27 falsos negativos e 923 verdadeiros negativos. Para regras sem alertas, a precisão é reportada como zero por `zero_division=0`.

## Reprodutibilidade e limites

As sementes e os parâmetros estão no código. As versões do ambiente original do Colab não foram preservadas neste arquivo exportado; por isso as dependências não estão fixadas em versões supostamente originais. Para registrar o ambiente de uma nova execução, use `python -m pip freeze > ambiente-execucao.txt` e registre também `python --version`.

Tempos variam entre máquinas e execuções. As dez repetições reutilizam a mesma base e não representam dez conjuntos independentes. Os modelos são ajustados e avaliados no próprio conjunto; não há avaliação em novos dados separados. A proporção real de anomalias é conhecida e fornecida aos modelos. Os resultados não demonstram superioridade universal de um método. Não foram medidos consumo de memória nem executados testes de duplicidades, Autoencoders ou modelos de linguagem.

## Origem

[Notebook original no Google Colab](https://colab.research.google.com/drive/17uG8Xn4j8msqUK5osTAoINLE7x7g-WdE) (acesso sujeito à permissão do proprietário).

Esta versão substitui a exibição específica do notebook por impressão no terminal e acrescenta exportação da matriz de confusão e dos tempos. A lógica e os parâmetros dos experimentos enviados foram preservados.
