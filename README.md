# Sistema de Dimensionamento Automático de Lesões (Protótipo do CHALLENGE DASA)

## Grupo
- Diego Cabral - RM 557817
- Débora Ivanowski - RM 555694
- Pedro Oliveira - RM 99943

## Descrição do Projeto
Este projeto é um protótipo desenvolvido em **Python** com execução pensada para ser executada no **Jupyter Notebook** ou em um terminal do **VS Code**.  
O sistema simula o fluxo de exames recebidos em um laboratório de patologia da **DASA** (Desafio 2), desde a triagem até a emissão de laudos com medições das lesões.  O objetivo é reproduzir, de forma simplificada, como a tecnologia pode automatizar etapas que hoje são feitas manualmente com régua física tornando o processo mais ágil e confiável.

Ao iniciar o programa, a fila de exames já aparece com valor igual a 5. Isso acontece porque o sistema já é carregado automaticamente com um lote inicial de exames fictícios definido na função carregar_lote_exames_do_dia(). Esse lote simula a situação real do laboratório, em que já existam exames aguardando triagem logo no começo do turno. De forma resumida, o número que aparece em fila representa a quantidade de exames feitos no dia que ainda não possuem laudo emitido.
Cada execução do programa começa, portanto, com 5 exames de exemplo (Ana, Bruno, Clara, Diego e Eva), para que as funcionalidades exigidas na documentação da sprint possam ser testadas assim que a execução do programa se inicia. 

Vale ressaltar que no sistema real do laboratório o fluxo lógico seria em duas etapas distintas:
	1.	O exame passa pela medição automática (realizada por IA);
	2.	Em seguida, o patologista valida os resultados e o sistema gera o laudo final.

No protótipo, essas duas etapas foram unificadas na opção 1 do menu:
	•	Ao escolher “Encaminhar próximo exame e emitir laudo”, o exame sai da fila, recebe imediatamente a medição simulada pela biblioteca random e já gera o laudo correspondente.

---

## Menu do Sistema
- `1` Encaminhar próximo exame e emitir laudo  
- `2` Consultar laudo por ID (busca sequencial)  
- `3` Consultar laudo por ID (busca binária)  
- `4` Listar laudos emitidos  
- `5` Desfazer último laudo emitido  
- `6` Ver topo da pilha de consultas  
- `0` Sair do programa  

---

## Estruturas e Algoritmos Utilizados

### 1) Fila de Exames (`FilaExamesDiarios`)
- Representa a ordem cronológica dos exames recebidos durante o dia.  
- Segue o modelo **FIFO** (First In, First Out).  
- Usada para garantir que os exames sejam processados na ordem de chegada.  

### 2) Pilhas (`PilhaConsultasRecentes` e `PilhaDesfazerProcessamento`)
- **Pilha de consultas**: armazena os últimos laudos consultados, priorizando o mais recente (LIFO).  
- **Pilha de desfazer**: guarda o histórico de laudos emitidos para permitir que se possa reverter o último e devolver o exame ao início da fila.  

### 3) Busca Sequencial
- Percorre todos os laudos até encontrar o ID desejado.  
- Complexidade **O(n)**.  
- Funciona em qualquer lista estando ela ordenada ou não.  
- Útil para conjuntos pequenos como no caso do código atual 

### 4) Busca Binária
- Divide a lista de laudos ao meio a cada passo.  
- Complexidade **O(log n)**.  
- Muito rápida, mas exige que a lista esteja **ordenada**.  
- Para isso, antes de usar a busca binária, o sistema verifica e ordena os laudos.  

### 5) Ordenação com Merge Sort
- Organiza os laudos por ID antes de consultas binárias.  
- Algoritmo sem variações, com complexidade **O(n log n)** em qualquer caso. 
- Neste protótipo foi implementado apenas o Merge Sort, pois atende ao requisito da documentação solicita. No entanto, em projetos reais, costuma-se combinar com outros algoritmos como o Insertion Sort em listas pequenas. 

### 6) Estimativa de Dimensão
- A função de medição gera um valor numérico simulando o dimensionamento do tecido.  
- No sistema real, essa etapa será substituída por um modelo de **visão computacional** (CNN)

---

## Complexidade Resumida
- Inserção e remoção na fila: **O(1)**  
- Operações de pilha (empilhar, desempilhar, topo): **O(1)**  
- Busca sequencial: **O(n)**  
- Busca binária: **O(log n)**  
- Ordenação (Merge Sort): **O(n log n)**  

---
## Função de desfazer (UNDO)
O sistema oferece a opção de Desfazer último laudo emitido.
Quando o usuário escolhe essa opção:
	•	O último laudo gerado é removido do repositório de laudos e o exame correspondente é devolvido para o início da fila de exames, como se nunca tivesse sido laudado. Esse recurso simula um cenário real no laboratório, em que um exame pode ter sido processado incorretamente e precisa ser revisto.
No protótipo, isso foi implementado usando uma pilha de desfazer (LIFO), que sempre guarda o último laudo emitido.

---
### Consulta de laudos por ID
Para consultar um laudo, o usuário precisa digitar o **ID** do exame. Só que esses IDs são carregados no início do programa ( 11, 13, 17) e isso dificulta saber que número digitar para realizar a busca. Para descobrir quais IDs estão disponíveis, basta usar a opção **4) Listar laudos** antes da consulta.  
No protótipo, foram usados IDs fictícios apenas para simulação. No real, cada exame terá seu identificador ou protocolo único vindo do banco de dados do laboratório.

---

## Bibliotecas utilizadas

O projeto utiliza apenas bibliotecas padrão do Python:
	•	collections.deque
	        Usada para implementar a fila de exames.
	        Permite inserção e remoção eficientes em ambas as extremidades da fila (O(1)).
	        Representa bem a ordem cronológica de chegada dos exames.

	•	random
            Usada para simular a medição das lesões em milímetros.
		    Nesse caso, irá gerar valores aleatórios representando o cálculo que no sistema real seria feito por uma IA de visão computacional (CNN)    

---

## Conexão com o Contexto do Problema
- A **fila** garante a ordem de chegada dos exames no laboratório.  
- A **pilha de consultas** permite que o patologista veja rapidamente os laudos mais recentes.  
- A **pilha de desfazer** permite corrigir erros recolocando exames para reprocessamento.  
- As **buscas** simulam duas formas de localizar laudos:  
  - simples (sequencial) para poucos casos,  
  - otimizada (binária) para muitos laudos já ordenados.   
- A **medição simulada** representa o papel da inteligência artificial no processo final.  

---

## Observações Finais 
- Trata-se de um protótipo que ainda está em desenvolvimento e será expandido para integrar a uma CNN real, banco de dados e a uma interface gráfica amigável. 
- Os nomes das funções e classes foram escolhidos para refletir semanticamente o problema (exames, laudos, fila, consultas, desfazer), e também facilitar a leitura do código por outros programadores. 