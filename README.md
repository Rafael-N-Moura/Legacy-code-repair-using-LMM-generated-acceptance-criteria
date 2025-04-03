# Legacy Code Repair Using LMM Generated Acceptance Criteria

Inspirado nos experimentos apresentados no estudo “Automated Repair of Programs from Large Language Models”, o projeto busca introduzir novas perspectivas e funcionalidades que visam aumentar a aplicabilidade e a eficácia das técnicas de reparo automático de código. Em vez de focar apenas em problemas competitivos, como os do LeetCode, a ideia central é concentrar os esforços em sistemas legados, onde a manutenção e evolução do código são desafios reais em ambientes corporativos.

Para isso, adaptando os experimentos originais para o contexto de código legado, usaremos LLMs como ferramentas de APR (mostradas, no artigo, como a alternativa mais eficiente de APR), o que permitirá avaliar como essas técnicas se comportam em cenários mais complexos e práticos. Além disso, o projeto integrará o uso de Large Language Models (LLMs) para gerar, de forma automatizada, critérios de aceitação baseados nas especificações e erros identificados no código. Essa abordagem visa não apenas orientar o processo de reparo automático, mas também fornecer um conjunto de requisitos de teste robustos que facilitem a validação das correções aplicadas.

Outro ponto importante que será é uma pipeline iterativa de reparo: a ideia é implementar um ciclo contínuo no qual a LLM gere inicialmente os critérios de aceitação e/ou sugestões de patch, modernize o sistema com base nos critérios gerados e quaisquer erros encontrados no novo código sejam realimentados para a LLM. Essa abordagem iterativa permite a evolução gradual do código, aprimorando tanto a funcionalidade quanto a legibilidade e manutenibilidade.

# Organização de pastas

- **Formatted code**: o código do sistema no formato enviado para a LLM; aquivos de compilação, configuração de IDE, e arquivos vazios, por exemplo, foram ignorados.
- **Pastas separadas por modelo**: organizar logs de conversas para testes e geração de código, segmentados em testes com descrições em linguagem natural.
- **Arquivos JAR**: arquivos para execução dos códigos modernizados.

Observação: Para execução dos arquivos JAR do projeto F-sharp é necessário possuir um servidor MySQL instalado, com usuário root e senha phokui. Crie também uma base de dados "music" com uma tabela "main". Sua estrura é a seguinte:

CREATE TABLE main (
    file_name VARCHAR(255) PRIMARY KEY,
    track VARCHAR(255),
    album VARCHAR(255),
    artist VARCHAR(255)
);


# Motivação

A modernização de sistemas legados são um _wicked problem_, um desafio de difícil especificação que possui um número ilimitado de estratégias de solução, tornando-se um problema persistente dentro da engenharia de software [1]. Sistemas legados estão presentes em muitas indústrias, a exemplo de instituições governamentais nos Estados Unidos, que possui sistemas legados que foram escritos até mesmo 60 anos atrás. Esses sistemas desatualizados podem causar sérios riscos à eficiência, segurança, e manutenção de código dessas instituições. Utilizando LLMs para modernização, no lugar das estatégias tradicionais, a reescrita manual do sistemas e refatoramento complexo baseado pode ser evitado, reduzindo custos e esforço de trabalho.

Visto que sistemas críticos respondem para quaisquer alterações, ainda que pequenas, também utilizamos a LLM para a geração de critérios de aceitação, permitindo que a LLM consiga inferir especificações não só do conjunto de código existente, aumentando a confiabilidade do código gerado. Assim, o projeto explora a interseção de técnicas de APR e do uso de LLMs para geração de critérios de aceitação. 

# Study Setting
Dada a complexidade de sistemas legados do mundo real, escolhemos projetos representativos desses sistemas, buscando equilibrar sua complexidade com nossa capacidade avaliá-los por completo. Ainda que não possuam complexidade comparável a sistemas coorporativos, dificuldades como documentação e implementação técnica desatualizada, e um certo nível de interdependência entre diferentes partes do códigos, são comuns a ambas as classes de sistema, permitindo que conclusões obtidas possam ser generalizadas para cenários reais. Esses projetos foram escritos em versões mais antigas da linguagem Java.

### Hipparcos-plot
Uma série de ferramenta da Agência Espacial Europeia escritas originalmente em 1997 e revisitadas 9 anos atrás. Seu módulo **Plot**, responsável pela geração de gráficos baseados em dados de entradas, foi escolhido para a modernização. Implementa também alguns conceitos de programação orientada a objeto.

### F-sharp
Um gerenciador de biblioteca musical integrado a um banco de dados MySQL. Escrito 16 anos atrás, esse projeto exemplicica como sistemas legados gerenciam conexões com base de dados e se adaptam para novos requisitos.

## Research Questions

Buscamos responder as seguintes questões:

### RQ1
Quão efetivas são LLMs na geração de critérios de aceitação para reparo de código?
### RQ2
Quão bem as técnicas de APR baseadas em LLMs funcionam em código legado?
### RQ3
Como o uso ou não uso dos testes dentro do prompt afetam o processo de reparo?

## LLM Models and Experimental Setup

Para a condução dos experimentos, usamos três LLMs, cada uma delas classificadas como _reasoning models_, capazes de atuar em tarefas complexas como solução de problemas matemáticos e geração de código. Os parâmetros padrão de cada modelo foram utilizados.

- GPT-3o-mini (Modelo 1)
- Deepseek R1 (Modelo 2)
- Gemini Flash Thinking (Modelo 3)

# Methodology
Nossa metodologia integra os critérios de aceitação com um processo iterativo de reparo automático de sistemas legado. Os experimentos podem ser divididos nas fases: geração de critério de aceitação, criação de LLMs para reparo, validação dos códigos gerados e refinimamento iterativo.

## Generation of Acceptance Criteria

Dois tipos de critério de aceitação são gerados:

### Natural Language Descriptions
A LLM é instruída a analisar o código fornecido, levando em consideração seu propósito, entradas e saídas esperadas, e gerar uma narrativa detalhada descrevendo todos os cenários necessários para verificar o comportamento correto do código. Essa narrativa atua como um conjunto de critérios de aceitação, e são inseridos no processo de modernização. O prompt para esta tarefa é: "Analyze the code below, including its purpose, inputs, and expected outputs. Then, generate a detailed natural language description of all the necessary scenarios that verify whether the code functions correctly and meets its intended requirements. Consider that the code will be transformed for the Java 17 language."

### Traditional Unit and Integration Tests:
De maneira análoga ao processo com linguagem natural, a LLM é instruída a gerar um conjunto de testes programáticos com o seguinte prompt:
"Analyze the code below, including its purpose, inputs, and expected outputs. Then, create a comprehensive test suite consisting of unit and/or integration tests that fully cover all aspects of the code's functionality, ensuring it produces a valid solution. Consider that the code will be transformed for the Java 17 language."

## LLM-Based Automated Repair Approaches

Uma vez que os critérios de aceitação são gerados, eles são usados para orientar o processo de correção. Testamos as seguintes abodagens:

### Test Suite Only Approach
O prompt inclui apenas os conjuntos de testes gerados e o código legado, enviados de maneira sequencial. Isso avalia a capacidade do LLM de corrigir o código com base exclusivamente em especificações programáticas de comportamento.

### Natural Language Description Only Approach
Apenas os critérios de aceitação em linguagem natural são fornecidos. Isso examina se requisitos descritivos podem guiar efetivamento o processo de correção sem casos de teste explícitos.

### Baseline Modernization Prompt
Um prompt simples é enviado sem nenhum critério de aceitação adicional: "Modernize the code to Java version 17". Essa abordagem serve para medir a eficácia dos critérios de aceitação na condução de correções precisas.

Para cada arquivo do sistema legado, os LLMs escolhidos (GPT-3o-mini, Deepseek R1 e Gemini Flash Thinking) são envolvidos com esses diferentes prompts, garantindo que o processo de correção seja avaliado em diversas estratégias de entrada.

## Iterative Repair and Validation Process

O processo de correção é executado em um ciclo iterativo que compreende as seguintes etapas:

### Modernization Candidate Generation:
Com base no prompt fornecido e nos critérios de aceitação, o LLM gera uma versão modernizada do código legado.

###  Testing and Validation:
É feita uma tentativa de compilar e executar o código.

### Feedback and Refinement:
Nos casos em que um patch candidato falha na execução, mensagens de erro e falhas nos testes são retornadas ao LLM. Um novo candidato é gerado e o teste é repetido.

## Evaluation Metrics and Reproducibility

Para medir a eficácia da nossa metodologia, consideramos:

### Acceptance Criteria Quality
Avaliado pela abrangência dos conjuntos de testes gerados e das descrições em linguagem natural, em comparação aos testes pensados por nós, além da adição de testes e sua cobertura de casos extremos.

### Modernization Quality
Avaliado pela facilidade de integração, sucesso/fracasso após a compilação e execução correta.

Todos os prompts, conjuntos de testes, e códigos modernizados são organizados dentro do repositório com pastas dedicadas (por exemplo, a pasta "gemini" para registros de conversas e geração de testes) para garantir total reprodutibilidade dos experimentos. Para uma leitura mais abrangente de nossa análise, consulte a apresentação em slides.

# References

- 1: Leveraging LLMs for Legacy Code Modernization: Challenges and Opportunities for LLM-Generated Documentation 
https://arxiv.org/pdf/2411.14971

- 2: HipparcosJava
https://github.com/esa/hipparcosJava/

- 3: F-Sharp
https://github.com/saahil/F-Sharp

- 4: Automatic Repair of Programs from Large Language Models
https://ieeexplore.ieee.org/document/10172854/
