# Quantum LS-SVM com Algoritmo HHL para Classificação de Dígitos MNIST

Este repositório contém a implementação do meu projeto de pesquisa em Computação Quântica aplicada à Inteligência Artificial (Ciência de Dados e Estatística - UFES). 

O objetivo principal deste projeto é demonstrar a viabilidade do uso de algoritmos quânticos para resolver problemas de aprendizado de máquina, especificamente utilizando o **Algoritmo HHL** (Harrow-Hassidim-Lloyd) para resolver o sistema linear gerado por um modelo **LS-SVM** (Least Squares Support Vector Machine).

## 🧠 Sobre o Projeto

Modelos clássicos de Support Vector Machine (SVM) com formulação de Mínimos Quadrados (LS-SVM) recaem na resolução de um sistema de equações lineares. Este projeto substitui o solucionador linear clássico pelo Algoritmo Quântico HHL, comparando a fronteira de decisão e a acurácia de ambas as abordagens.

Devido às limitações de hardware da **Era NISQ** (Noisy Intermediate-Scale Quantum) — como a escassez de qubits e o alto índice de ruído e descoerência —, não é possível processar imagens em alta resolução diretamente no circuito quântico. Para contornar isso, foi desenvolvida uma técnica clássica de extração de características espaciais que reduz drasticamente a dimensionalidade dos dados sem perder o poder de separabilidade.

## ⚙️ Pipeline de Dados e Metodologia

O fluxo do projeto é dividido em uma etapa de pré-processamento clássico e uma etapa de execução quântica:

### 1. Pré-processamento Clássico (Extração HR/VR)
* **Dataset:** Subconjunto do MNIST focado na classificação binária dos dígitos **6** e **9**.
* **Redução de Dimensionalidade:** As imagens originais (vetores de tamanho 784 ou matrizes 28x28) foram reduzidas para uma matriz de características 2x2.
* **Heurística:** Foram calculadas as Razões Horizontais (HR) e Razões Verticais (VR) da densidade de pixels pretos (dividindo a imagem ao meio vertical e horizontalmente). Essa técnica captura a assimetria natural dos números (ex: o 6 tem mais massa embaixo, o 9 em cima).

### 2. Formulação do LS-SVM
Com os dados condensados na matriz 2x2, calculamos os vetores médios das classes e formulamos o sistema linear regularizado do LS-SVM clássico, dado por:

$$ 
A = K + \gamma^{-1} I
$$

Onde $K$ é a matriz de Kernel e $\gamma^{-1} I$ é o fator de penalização/regularização. Esta matriz $A$, juntamente com o vetor de rótulos, forma o sistema que será enviado ao computador quântico.

### 3. Execução Quântica (Algoritmo HHL)
A matriz $A$ e o vetor alvo são processados no circuito quântico seguindo 4 etapas fundamentais:
1. **Preparação de Estados:** Codificação do vetor de rótulos nas amplitudes do registrador de input.
2. **Estimativa de Fase Quântica (QPE):** Extração dos autovalores da matriz $A$ para um registrador de *clock*.
3. **Rotação Controlada:** Aplicação de uma rotação no qubit *Ancilla* proporcional ao inverso dos autovalores ($2 \arcsin(C/\lambda)$).
4. **Desfazer o Cômputo (Uncomputing):** QPE inversa para desmaranhar o registrador de *clock*.
5. **Medição:** Medição do qubit Ancilla com pós-seleção no estado $|1\rangle$, colapsando o registrador principal no vetor solução do sistema.

## 📊 Resultados

O modelo quântico obteve resultados extremamente satisfatórios, comprovando a eficácia matemática do HHL como prova de conceito para o LS-SVM:

| Modelo | Vetor Solução (Pesos) | Acurácia |
| :--- | :--- | :--- |
| **LS-SVM Clássico** | $(1.98, -1.98)$ | **90.85%** |
| **LS-SVM Quântico (HHL)** | $(0.70, -0.71)$ * | **90.85%** |

*\*Nota: O vetor solução quântico é numericamente menor pois os estados quânticos são naturalmente normalizados (norma 1). No entanto, a proporção geométrica é idêntica à clássica, resultando no mesmo hiperplano de separação e, consequentemente, na mesma acurácia.*

## 🚀 Conclusão

Embora o atual limite físico (ruído e coerência) dos computadores quânticos impeça a entrada de dados em larga escala, a estratégia de compressão por densidade espacial (HR/VR) provou ser uma ponte eficiente entre o processamento clássico e o quântico. Os resultados idênticos de acurácia (90,85%) demonstram o promissor potencial de vantagem computacional do LS-SVM Quântico no longo prazo, quando hardwares tolerantes a falhas permitirem o processamento de grandes volumes de dados de forma exponencialmente mais rápida.

---
**Autor:** Thierry  
**Instituição:** Universidade Federal do Espírito Santo (UFES) - Estatística e Ciência de Dados.
