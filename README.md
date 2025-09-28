# Estudo de Hierarquia de Memória - ShowMemory

- Falta:
    - Fazer um arquivo `TR.txt`, para testar os cenários sob as mesmas condições.
    - Testar e avaliar os resultados.
    - Fazer o artigo do [Overleaf](#Overleaf).

## Objetivo

O trabalho consiste em simular arquiteturas de memória no simulador Amnesia e analisar como diferentes parâmetros influenciam o desempenho. A ideia é validar os conceitos de **localidade temporal** (reuso de dados recentemente acessados) e **localidade espacial** (acesso a dados próximos na memória).

Para isso, foram criados **seis cenários de simulação**: três explorando apenas **variações na cache** e três explorando apenas **variações na memória virtual**. Assim, isolamos o impacto de cada parte da hierarquia de memória.

## Cenários de Simulação

### Cenários de Cache (memória virtual padronizada)

- **Arquitetura-Cenario-Cache-1-(L4B-C64B-1W-WB-LRU).xml**

    - Cache pequena (64B)
    - Direto mapeado (1-way)
    - WriteBack + LRU
    - Objetivo: avaliar o comportamento básico de uma cache mínima, onde o impacto de conflitos de mapeamento e capacidade é mais evidente.

- **Arquitetura-Cenario-Cache-2-(L8B-C128B-2W-WT-FIFO).xml**

    - Cache média (128B)
    - 2-way associativo
    - WriteThrough + FIFO
    - Objetivo: observar como associatividade reduz conflitos e como a política de escrita WriteThrough influencia no tráfego de memória.

- **Arquitetura-Cenario-Cache-3-(L16B-C256B-4W-WB-LRU).xml**

    - Cache maior (256B)
    - 4-way associativo
    - WriteBack + LRU
    - Objetivo: explorar ganhos de desempenho com blocos maiores e mais associatividade, favorecendo localidade espacial e reduzindo taxas de miss.

### Cenários de Memória Virtual (cache padronizada)

- **Arquitetura-Cenario-VM-1-(PS32-D128-TLB2-FIFO).xml**

    - Página de 32 palavras
    - Disco de 128 palavras
    - TLB pequena (2 entradas)
    - Substituição FIFO na tabela de páginas
    - Objetivo: avaliar comportamento com páginas pequenas e baixa cobertura da TLB, resultando em mais page faults.

- **Arquitetura-Cenario-VM-2-(PS64-D256-TLB4-LRU).xml**

    - Página de 64 palavras
    - Disco de 256 palavras
    - TLB média (4 entradas)
    - Substituição LRU na tabela de páginas
    - Objetivo: analisar impacto de páginas maiores e de uma TLB um pouco mais robusta, favorecendo localidade temporal.

- **Arquitetura-Cenario-VM-3-(PS128-D512-TLB8-NRU).xml**

    - Página de 128 palavras
    - Disco de 512 palavras
    - TLB maior (8 entradas)
    - Substituição NRU na tabela de páginas
    - Objetivo: simular um sistema mais robusto, explorando localidade espacial com páginas grandes e comparando a política NRU frente ao LRU.

## Padronizações Adotadas

Para garantir que os resultados comparados refletissem apenas os parâmetros de interesse (cache ou memória virtual), foram feitas as seguintes padronizações:

- **MainMemory**

    - Tamanho fixo de 1024B
    - `ciclesPerAccessRead = 1`, `ciclesPerAccessWrite = 2`, `timeCicle = 1`
    - O `blockSize` sempre igual ao `lineSize` da cache, garantindo consistência no mapeamento.

- **Cache** (para cenários de memória virtual)

    - Cache unificada
    - 256B, 4-way associativa, WriteBack + LRU
    - Ciclos padronizados.

- **VirtualMemory** (para cenários de cache)

    - Página de 32 palavras, disco de 128 palavras
    - TLB unificada com 4 entradas
    - Política LRU
    - Ciclos padronizados (`read=10`, `write=20`, `timeCicle=100` para disco).

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

## Overleaf

- Modelo       : [https://www.overleaf.com/latex/templates/ieee-conference-template/grfzhhncsfqn](https://www.overleaf.com/latex/templates/ieee-conference-template/grfzhhncsfqn)
- Visualização : [https://www.overleaf.com/read/rvygqcfmtfsh#7c38c4](https://www.overleaf.com/read/rvygqcfmtfsh#7c38c4)
- Edição       : [https://www.overleaf.com/4121384261rzntktrcvsng#99be3e](https://www.overleaf.com/4121384261rzntktrcvsng#99be3e)

## Integrantes

- André Luís Silva de Paula
- Caio Faria Diniz
- Giuseppe Sena Cordeiro
- Vinícius Miranda de Araújo
