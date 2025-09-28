# Estudo de Hierarquia de Memória - ShowMemory

## Objetivo

O trabalho consiste em simular arquiteturas de memória no simulador Amnesia e analisar como diferentes parâmetros influenciam o desempenho. A ideia é validar os conceitos de **localidade temporal** (reuso de dados recentemente acessados) e **localidade espacial** (acesso a dados próximos na memória).

Para isso, foram criados **seis cenários de simulação**: três explorando apenas **variações na cache** e três explorando apenas **variações na memória virtual**. Assim, isolamos o impacto de cada parte da hierarquia de memória.

## Cenários de Simulação

### Cenários de Cache (memória virtual padronizada)

- **Arquitetura-Cenario-Cache-1-(L4B-C64B-1W-WB).xml**
  - Cache pequena (64B)
  - Direto mapeado (1-way)
  - WriteBack + LRU
  - **Objetivo**: avaliar o comportamento de uma cache mínima, onde conflitos de mapeamento e limitação de capacidade impactam diretamente na taxa de miss.

- **Arquitetura-Cenario-Cache-2-(L8B-C128B-2W-WT-FIFO).xml**
  - Cache média (128B)
  - 2-way associativo
  - WriteThrough + FIFO
  - **Objetivo**: analisar como a associatividade reduz conflitos e como a política de escrita *WriteThrough* aumenta o tráfego de memória, além do efeito de um algoritmo de substituição mais simples (FIFO).

- **Arquitetura-Cenario-Cache-3-(L16B-C256B-4W-WB-LRU).xml**
  - Cache maior (256B)
  - 4-way associativo
  - WriteBack + LRU
  - **Objetivo**: explorar ganhos de desempenho com linhas maiores (16B) e maior associatividade, favorecendo localidade espacial e reduzindo taxas de miss.

### Cenários de Memória Virtual (cache padronizada)

- **Arquitetura-Cenario-VM-1-(PS32-D128-TLB2-FIFO).xml**

  - Página de 32 palavras
  - Disco de 128 palavras
  - TLB pequena (2 entradas)
  - Substituição FIFO
  - **Objetivo**: avaliar comportamento de páginas pequenas e baixa cobertura de TLB, resultando em mais *page faults* e mais acessos ao disco.

- **Arquitetura-Cenario-VM-2-(PS64-D256-TLB4-LRU).xml**

  - Página de 64 palavras
  - Disco de 256 palavras
  - TLB média (4 entradas)
  - Substituição LRU
  - **Objetivo**: analisar impacto de páginas maiores (melhor localidade espacial) e de uma TLB mais robusta, explorando localidade temporal com LRU.

- **Arquitetura-Cenario-VM-3-(PS128-D512-TLB8-NRU).xml**

  - Página de 128 palavras
  - Disco de 512 palavras
  - TLB grande (8 entradas)
  - Substituição NRU
  - **Objetivo**: simular sistema mais robusto, com maior aproveitamento da localidade espacial, mas avaliando o efeito de uma política de substituição menos precisa (NRU).

## Padronizações Adotadas

Para garantir que os resultados comparados refletissem apenas os parâmetros de interesse (cache ou memória virtual), foram feitas as seguintes padronizações:

- **<MainMemory>**
  - Presente em todos os cenários.
  - Tamanho fixo de 1024B.
  - Ciclos padronizados (`read = 1`, `write = 2`, `timeCicle = 1`).
  - Nos **cenários de cache**: `blockSize = lineSize` da cache.
  - Nos **cenários de VM**: `blockSize = 1`.

- **<Cache>**
  - Presente apenas nos **cenários de cache** (removida dos cenários de VM).
  - Ciclos padronizados (`read = 1`, `write = 2`, `timeCicle = 1`).

- **<VirtualMemory>**
  - Presente apenas nos **cenários de VM** (removida dos cenários de cache).
  - Ciclos de Disco padronizados (`read = 10`, `write = 20`, `timeCicle = 100`).
  - TLB unificada.
  - Ciclos da TLB padronizados (`read = 1`, `write = 2`, `timeCicle = 1`).

Essa padronização garante que, em cada grupo de cenários, apenas a dimensão estudada (cache ou memória virtual) varie, isolando os efeitos no desempenho.

## Padrão de Nome de Arquivo

```
Arquitetura-Cenario-[Tipo]-X-(Parâmetros).xml
```

Onde:

- **Tipo**     $\to$ Cache ou VM
- **LxB**      $\to$ tamanho da linha da cache em bytes
- **CyB**      $\to$ tamanho total da cache em bytes
- **zW**       $\to$ associatividade (`1W`, `2W`, `4W`, etc.)
- **PolWrite** $\to$ política de escrita (WB ou WT)
- **PolSub**   $\to$ política de substituição (LRU, FIFO, NRU)
- **PSn**      $\to$ tamanho da página (n palavras)
- **Dn**       $\to$ tamanho do disco (n palavras)
- **TLBn**     $\to$ tamanho da TLB

## Artigo

- Modelo       : [IEEE Conference Template](https://www.overleaf.com/latex/templates/ieee-conference-template/grfzhhncsfqn)
- Visualização : [https://www.overleaf.com/read/rvygqcfmtfsh#7c38c4](https://www.overleaf.com/read/rvygqcfmtfsh#7c38c4)
- Edição       : [https://www.overleaf.com/4121384261rzntktrcvsng#99be3e](https://www.overleaf.com/4121384261rzntktrcvsng#99be3e)

## Slides

- Visualização : [https://www.overleaf.com/read/qycrgjdpzwts#22cdd6](https://www.overleaf.com/read/qycrgjdpzwts#22cdd6)
- Edição       : []()

## Integrantes

- André Luís Silva de Paula
- Caio Faria Diniz
- Giuseppe Sena Cordeiro
- Vinícius Miranda de Araújo
