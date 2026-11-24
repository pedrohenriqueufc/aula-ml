# ATP07 — Versionamento de Dados e Modelos

Este repositório apresenta um experimento de versionamento de dados e modelos utilizando Git e DVC, com o objetivo de demonstrar a rastreabilidade dos conjuntos de dados, a reprodução dos treinamentos e a comparação entre diferentes versões.

## Pré-requisitos

- Git instalado.
- Python 3 instalado.
- DVC instalado.
- Acesso ao repositório GitHub.
- Acesso ao armazenamento utilizado pelo DVC.

Instalação do DVC:

```bash
pip install dvc
```

Verifique as instalações:

```bash
git --version
python3 --version
dvc --version
```

## Estrutura do projeto

```text
aula-ml/
├── data/
│   ├── chamados.csv
│   ├── chamados.csv.dvc
│   ├── split.csv
│   └── split.csv.dvc
├── reports/
│   └── comparacao.md
├── src/
│   └── train.py
├── dvc.yaml
├── README.md
└── .gitignore
```

## Configuração do armazenamento DVC

O projeto utiliza um armazenamento local separado do repositório Git, localizado no diretório `dvc-storage`, ao lado da pasta `aula-ml`.

Estrutura esperada:

```text
diretorio/
├── aula-ml/
└── dvc-storage/
```

Após clonar o repositório, configure o armazenamento:

```bash
dvc remote add -d storage ../dvc-storage
```

Caso o remoto já esteja cadastrado, atualize seu endereço:

```bash
dvc remote modify storage url ../dvc-storage
```

Para verificar a configuração:

```bash
dvc remote list
```

**Importante:** o armazenamento DVC precisa estar disponível no caminho configurado. O GitHub armazena os arquivos de metadados `.dvc`, mas não substitui o armazenamento dos dados versionados.

## Restaurando uma versão do experimento

Para restaurar a versão inicial dos dados e do modelo:

```bash
git checkout dados-v1
dvc pull
python3 src/train.py chamados-r1
```

Para restaurar a segunda versão:

```bash
git checkout dados-v2
dvc pull
python3 src/train.py chamados-r2
```

Os comandos `git checkout` e `dvc pull` permitem recuperar, respectivamente, os metadados versionados e os arquivos armazenados pelo DVC.

## Verificando as versões

Para visualizar as tags disponíveis:

```bash
git tag
```

Para comparar os metadados dos conjuntos de dados:

```bash
git diff dados-v1 dados-v2 -- data/chamados.csv.dvc
```

Para comparar as partições:

```bash
git diff dados-v1 dados-v2 -- data/split.csv.dvc
```

A partição de treinamento e teste deve permanecer fixa durante a comparação, garantindo que as métricas sejam obtidas sobre os mesmos registros.

## Reprodutibilidade

A execução dos treinamentos deve utilizar a mesma implementação, a mesma partição de teste e as versões de dados correspondentes a cada experimento.

Os resultados e a decisão sobre a aceitação da segunda versão estão documentados em `reports/comparacao.md`.
