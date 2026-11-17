# Uso de IA generativa e retenção de conhecimento em estudantes universitários

Projeto final da disciplina **MCEQ 12576 – Métodos de Computação Experimental e Quantitativa**
PUC-Campinas · Engenharia de Software · 2026

**Equipe:** Gabriel Angeli Capovilla (RA 24003637) e Henrique Della Guardia de Azevedo Martins (RA 24003251)

## Pergunta de pesquisa

Em estudantes universitários, usar IA generativa (ChatGPT) para responder perguntas de prática, em vez de respondê-las sozinho, altera a retenção do conteúdo no teste final?

- **H0:** μ_genAI = μ_self (proporção média de acertos no teste final igual nas duas condições)
- **H1 (bilateral):** μ_genAI ≠ μ_self
- **Desenho:** pareado (cada participante passa pelas duas condições), n = 34
- **Teste previsto:** t pareado, α = 0,05; alternativo: Wilcoxon pareado

O plano completo, definido antes da análise, está em [`docs/protocolo.pdf`](docs/protocolo.pdf).

## Origem dos dados

Heshmati, H.; Tullis, J. G. *Using Generative AI to Answer Practice Questions Impairs Long Term Retention*. Zenodo, versão v1, 2026. DOI: [10.5281/zenodo.23749606](https://doi.org/10.5281/zenodo.23749606)

| Item | Valor |
| --- | --- |
| Autores | Hosain Heshmati e Jonathan G. Tullis (University of Arizona) |
| Publicação | 29/10/2026, versão v1 |
| Data de acesso | 16/11/2026 |
| Licença | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), que permite redistribuir os arquivos com atribuição |
| Arquivos | `experiment1_data.xlsx` (MD5 `d57f7f30a4429b9580e2f95c10353210`) e `experiment2_data.xlsx` (MD5 `fe8edd191df479292ad25fc2ec6435fc`), idênticos aos do Zenodo |

Os arquivos em `dados/brutos/` são cópias sem nenhuma alteração.

### Dicionário de variáveis (interpretado pela equipe)

A base não traz dicionário de variáveis nem artigo associado. O significado abaixo foi inferido pela equipe a partir da estrutura dos dados e da descrição curta do registro no Zenodo:

| Coluna | Significado | Valores |
| --- | --- | --- |
| `subject_id` | Participante | E1P001 a E1P034 (Exp. 1); E2P001 a E2P045 (Exp. 2) |
| `topic` | Texto lido | volcano, food, ice, money |
| `condition` | Como as perguntas de prática foram respondidas | Exp. 1: `genAI`, `self`. Exp. 2: `genAI_simple`, `genAI_elaborated`, `self_simple`, `self_elaborated` |
| `item_number` | Número da pergunta no texto | 1 a 10 |
| `question_type` | Tipo de pergunta | `factual`, `inference` |
| `initial_correct` | Acerto na prática (dia 1) | 0 ou 1 |
| `test_correct` | Acerto no teste final (**retenção, VD**) | 0 ou 1 |
| `passage_jol` | Julgamento de aprendizagem do texto | 1 a 10 |
| `item_jol` | Julgamento de aprendizagem da pergunta | 1 a 4 |
| `day1_response_time_sec` | Tempo de resposta na prática | segundos |

### Limitações da base

- **Participantes não descritos:** o registro não informa quem eram os participantes (perfil, curso, idade, universidade). Assumimos, sem confirmação, que são estudantes universitários, pelo contexto do laboratório da University of Arizona.
- **Intervalo do teste final:** o intervalo entre a prática e o teste final **não foi informado pelos autores**.
- **Sem dicionário:** o significado das colunas foi inferido pela equipe (tabela acima).

## Estrutura do repositório

```
README.md               este arquivo
requirements.txt        bibliotecas necessárias
analise.ipynb           análise completa, do dado bruto ao resultado
dados/brutos/           dados originais do Zenodo (nunca editados)
dados/limpos/           tabelas geradas pelo notebook
figuras/                figuras geradas pelo notebook
docs/protocolo.pdf      protocolo da análise (17/11/2026)
```

## Como reexecutar

Requer Python 3.10 ou superior.

```bash
git clone <URL deste repositório>
cd mceq-ia-retencao
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook analise.ipynb   # depois: Run All
```

O notebook regrava todos os arquivos de `dados/limpos/` e `figuras/`. Nenhuma etapa usa sorteio, então os números saem iguais em qualquer execução.

## O que o notebook faz

1. Lê o arquivo bruto e confere a integridade: 34 participantes, 40 itens cada, 2 textos por condição, nenhum dado faltante.
2. Calcula, para cada participante, a proporção de acertos no teste final em cada condição. Nenhuma observação é excluída.
3. Gera a tabela descritiva.
4. Gera o gráfico pareado com as médias e IC 95%.
5. Verifica a normalidade das diferenças (Shapiro-Wilk, histograma e Q-Q plot).
6. Roda o teste confirmatório: t pareado, ou Wilcoxon se a normalidade falhar, com tamanho de efeito d_z e IC 95%.
7. Faz análises **exploratórias**, só descritivas: resultados por tipo de pergunta e do Experimento 2.

## Licença

- **Dados:** CC BY 4.0. Autoria de Heshmati e Tullis (2026); ver a citação acima.
- **Código:** livre para uso acadêmico.