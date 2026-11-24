# Comparação entre as versões dos dados

## 1. Alteração realizada

Na versão `dados-v1`, o registro de ID `006`, referente ao chamado "bloqueio de acesso", estava incorretamente classificado como `comum`.

Na versão `dados-v2`, esse registro foi corrigido para a classe `urgente`.

A alteração foi realizada mantendo a mesma partição de treinamento e teste. Durante a verificação, também foram removidas duas linhas vazias extras que existiam no arquivo `split.csv` da segunda versão. A tag `dados-v2` foi atualizada para registrar os metadados corrigidos.

## 2. Comparação das métricas

Os modelos foram treinados utilizando as respectivas versões dos dados e a mesma partição de teste.

| Métrica | Modelo r1 | Modelo r2 |
|---|---:|---:|
| Acurácia | 75% | 100% |
| Recall — urgentes | 50% | 100% |
| Recall — comuns | 100% | 100% |

A segunda versão apresentou melhora no recall da classe `urgente`, sem reduzir o recall da classe `comum`.

## 3. Verificação dos hashes

Os hashes registrados nos arquivos `.dvc` foram comparados com os hashes calculados sobre os arquivos restaurados em um clone limpo do repositório.

| Arquivo | Hash — dados-v1 | Hash — dados-v2 |
|---|---|---|
| `data/chamados.csv` | `c54cd50d5b9e77a1b46544afda3b13b0` | `bd856dc82f2847a61b46c44c5da8d6ab` |
| `data/split.csv` | `859050722ed50484365f694d77f3c105` | `859050722ed50484365f694d77f3c105` |

A restauração das duas versões foi realizada com sucesso utilizando o DVC. Os hashes calculados no clone limpo coincidiram integralmente com os valores registrados nos metadados.

A igualdade dos hashes de `split.csv` confirma que a partição de treinamento e teste permaneceu idêntica nas duas versões. A diferença nos hashes de `chamados.csv` corresponde à correção do rótulo do chamado 006.

Dessa forma, a integridade dos dados e a capacidade de recuperação das versões foram verificadas.

## 4. Decisão

**Decisão: aceitar a versão r2.**

A versão `dados-v2` atende ao critério estabelecido para o exercício: melhorar o recall dos chamados urgentes sem reduzir o recall dos chamados comuns.

O recall de urgentes passou de 50% para 100%, enquanto o recall de comuns permaneceu em 100%.

Como a partição de teste foi mantida entre os experimentos, a comparação permite avaliar o impacto da correção do rótulo sem alterar a composição dos dados utilizados na avaliação.

## 5. Conclusão

O experimento demonstrou a importância do versionamento de dados e da rastreabilidade dos modelos.

A utilização conjunta de Git e DVC permitiu registrar as versões dos arquivos, recuperar os conjuntos de dados e reproduzir os treinamentos.

A versão `r2` foi considerada superior para este exercício, pois apresentou melhora na identificação de chamados urgentes sem prejudicar a identificação de chamados comuns.
