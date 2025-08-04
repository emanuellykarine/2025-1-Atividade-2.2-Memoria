# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositótio do aluno: [Emanuelly Karine F. dos Santos](https://github.com/emanuellykarine)

## Tarefas do aluno
1. Fork desse repositório e atualizar a linha 10 com o nome e link do github
2. Ler a descrição da atividade
3. Montar a resposta no final deste arquivo, no tópico **Resposta**

---

## 1. Descrição da atividade
### 1.1. Objetivo
Praticar os conceitos de alocação de memória (best-fit), memória virtual e desfragmentação em um sistema com memória limitada.

---

### 1.2. Contexto
Um computador possui apenas **64 KB de RAM** e um **disco rígido para memória virtual**. O sistema operacional deve gerenciar 5 processos com tamanhos diferentes, cuja soma ultrapassa a capacidade da RAM.

#### 1.2.1. Processos iniciais

| Processo | Tamanho (KB) |
|----------|-------------|
| P1       | 20          |
| P2       | 15          |
| P3       | 25          |
| P4       | 10          |
| P5       | 18          |
| **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  
- **Memória Virtual (Disco)**: Espaço ilimitado para paginação.

#### 1.2.2. Alocação Inicial com Best-Fit
Os alunos devem simular a alocação dos processos na RAM usando o algoritmo **best-fit**.  
- A memória RAM será representada como um bloco contíguo (ex: `[0KB - 64KB]`).  
- Devem alocar os processos nos menores espaços livres que atendam ao seu tamanho.  

**Alocação inicial**:  
1. P1 (20 KB) → Ocupa [0-20].  
2. P2 (15 KB) → Ocupa [20-35].  
3. _continuar a partir daqui_

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.  

#### 1.2.4. Desfragmentação da RAM
- Desfragmentar a RAM para liberar espaço contíguo.
- Após desfragmentação (compactação), verificar quais processos podem ser alocado.  

### 1.3. Questões para Reflexão
1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?  
2. Como a memória virtual evitou um deadlock?  
3. Qual o impacto da desfragmentação no desempenho do sistema?  

---

## Resposta

### 1. Alocação Inicial com Best-Fit
- Com o algoritmo Best-fit (melhor encaixe) precisamos alocar os processos em blocos de memória disponíveis que deixem o menor espaço possível sem utilização. Então com as especificações que temos: 

    | Processo | Tamanho (KB) |
    |----------|-------------|
    | P1       | 20          |
    | P2       | 15          |
    | P3       | 25          |
    | P4       | 10          |
    | P5       | 18          |
    | **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  

A alocação ficará da seguinte forma:
1. P1 (20 KB) - [0-20] (sobra 44 KB na RAM)
2. P2 (15 KB) - [20-35] (sobra 29 KB na RAM)
3. P3 (25 KB) - [35-60] (sobra 4 KB na RAM)
4. P4 (10 KB) - Não cabe em nenhum espaço livre (4 KB). Vai para a memória virtual.
5. P5 (18 KB) - Não cabe no espaço livre. Vai para a memória virtual.

- Situação final da RAM após best-fit:
    RAM:
    
    P1: [0 - 20)
    
    P2: [20 - 35)
    
    P3: [35 - 60)
    
    Livre: [60 - 64) = 4 KB

    Memória virtual (disco):
    
    P4 (10 KB)
    
    P5 (18 KB)

### 2. Simular Memória Virtual (Paginação)
- A paginação se dará da seguinte forma:
    | Processo | Tamanho (KB) | Onde estão    |
    |----------|--------------|---------------|
    | P1       | 20           |RAM            |
    | P2       | 15           |RAM            |
    | P3       | 25           |RAM e Disco    | 
    | P4       | 10           |Disco          | 
    | P5       | 18           |Disco          |

- Como a RAM só possui 64 KB, o restante de P3 e os demais depois dele precisam ser paginados no disco. Já que P1, P2 e parte de P3 já preencheu quase toda a RAM.

### 3. Desfragmentação da RAM
- Após a desfragmentação o espaço contíguo continua sendo [60-64] = 4 KB pois não há buracos internos na RAM, somente um espaço no final. A desfragmentação ocorre simplesmente reorganizando os processos compactando-os se necessário, mas no exemplo eles já estão organizados.
- Com isso nenhum processo pode ser alocado, pois o espaço (4 KB) ainda não é suficiente para P4 (10 KB) e nem P5 (18 KB).

 ### 4. Questões para Reflexão
**1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?**

    O algoritmo worst-fit é uma estratégia que tenta encontrar a maior partição livre disponível e alocar o processo a essa partição. Em comparação a esse, o algoritmo best-fit é mais eficiente pois evita grandes fragmentações e aproveita melhor os blocos disponíveis ao invés de procurar especificamente a maior partição livre disponível. Já o first-fit é a estratégia que escolhe o primeiro bloco disponível que suporte o processo, mas no exemplo acima a funcionalidade acaba sendo a mesma, visto que os blocos foram preenchidos em sequência.

**2. Como a memória virtual evitou um deadlock?**

    Um deadlock ocorre quando dois ou mais processos ficam bloqueados, esperando que um libere um recurso que o outro está utilizando. A memória virtual permite que processos que não cabem totalmente na RAM sejam armazenados no disco, o que possibilita que o sistema continue funcionando e executando os processos por meio da paginação.

**3. Qual o impacto da desfragmentação no desempenho do sistema?**

    A desfragmentação melhora a utilização da memória, permitindo que novos processos sejam carregados caso haja espaço total suficiente, mas distribuído de forma fragmentada. No aspecto negativo, a desfragmentação consome tempo e recursos, pois os dados precisam ser copiados de um lugar para outro na RAM.
