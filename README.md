# Estudo de Hierarquia de Memória

- Falta:
    - Fazer um arquivo `TR.txt`, para testar os cenários sob as mesmas condições.
    - Testar e avaliar os resultados.
    - Fazer o artigo do [Overleaf](#Overleaf).

## Objetivo

O trabalho consiste em simular arquiteturas de memória cache no simulador Amnesia e analisar como diferentes parâmetros influenciam o desempenho. A ideia é validar os conceitos de localidade temporal (reuso de dados recentemente acessados) e localidade espacial (acesso a dados próximos na memória).

Ou seja, variar configurações do sistema de memória e observar taxas de hit/miss e tempo médio de acesso.

## Cenários de Simulação

- Arquitetura-Cenario-1-(L1B-C64B-1W-WB-LRU).xml

    - Cache pequena (64B)
    - Direto mapeado (1-way)
    - WriteBack + LRU
    - Objetivo: comparar comportamento básico de cache mínima.

- Arquitetura-Cenario-2-(L4B-C128B-2W-WT-FIFO).xml

    - Cache média (128B)
    - 2-way associativo
    - WriteThrough + FIFO
    - Objetivo: observar impacto de WriteThrough e política FIFO.

- Arquitetura-Cenario-3-(L8B-C256B-4W-WB-FIFO).xml

    - Cache maior (256B)
    - 4-way associativo
    - WriteBack + FIFO
    - Objetivo: explorar mais associatividade e blocos maiores.

- Arquitetura-Cenario-4-(Split-I32B-D32B-2W-WT-LRU).xml

    - Cache Split (32B para instruções + 32B para dados)
    - 2-way associativo
    - WriteThrough + LRU
    - Objetivo: avaliar impacto de cache separada em cargas mistas.

- Arquitetura-Cenario-5-(L16B-C512B-8W-WB-LRU-TLB4).xml

    - Cache grande (512B)
    - 8-way associativo
    - WriteBack + LRU
    - TLB maior (4 entradas)
    - Objetivo: simular arquitetura mais robusta, com alto grau de associatividade e suporte de TLB.

### Padronização dos Ciclos

Durante a construção dos cenários, foi feita a padronização dos parâmetros de ciclos (ciclesPerAccessRead, ciclesPerAccessWrite, timeCicle) para que a análise se concentre apenas nas variações estruturais da hierarquia de memória (tamanho de cache, associatividade, política de escrita e substituição, etc.).

A padronização foi definida da seguinte forma:

- Memória principal e cache:
    - ciclesPerAccessRead = 1
    - ciclesPerAccessWrite = 2
    - timeCicle = 1

- Memória virtual (disco):
    - ciclesPerAccessRead = 10
    - ciclesPerAccessWrite = 20
    - timeCicle = 100

Essa padronização permite que os resultados obtidos reflitam apenas as diferenças de projeto das caches, sem influência de tempos artificiais de leitura/escrita.

### Padrão de Nome de Arquivo

```
Arquitetura-Cenario-X-(LxB-CyB-zW-PolSub-PolWrite[-TLBn]).xml
```

Onde:

- LxB      $\to$ tamanho da linha da cache em bytes
- CyB      $\to$ tamanho total da cache em bytes
- zW       $\to$ associatividade (`1W` = direto mapeado, `2W` = 2-way, etc.)
- PolSub   $\to$ política de substituição (`LRU` ou `FIFO`)
- PolWrite $\to$ política de escrita (`WriteBack` = `WB`, `WriteThrough` = `WT`)
- TLBn     $\to$ (opcional) tamanho da `TLB`

## Overleaf

- Modelo       : https://www.overleaf.com/latex/templates/ieee-conference-template/grfzhhncsfqn
- Vizualização : https://www.overleaf.com/read/rvygqcfmtfsh#7c38c4
- Edição       : https://www.overleaf.com/4121384261rzntktrcvsng#99be3e

## Integrantes

- André Luís Silva de Paula
- Caio Faria Diniz
- Giuseppe Sena Cordeiro
- Vinícius Miranda de Araújo
