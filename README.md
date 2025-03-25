# Legacy Code Repair Using LMM Generated Acceptance Criteria

O projeto propõe reavaliar e expandir os experimentos apresentados no estudo “Automated Repair of Programs from Large Language Models”, introduzindo novas perspectivas e funcionalidades que visam aumentar a aplicabilidade e a eficácia das técnicas de reparo automático de código. Em vez de focar apenas em problemas competitivos, como os do LeetCode, a ideia central é concentrar os esforços em sistemas legados, onde a manutenção e evolução do código são desafios reais em ambientes corporativos.

Para isso, inspirado nos experimentos originais e adaptando-os para o contexto de código legado, usaremos LLMs (mostradas, no artigo, como a alternativa mais eficiente de APR), o que permitirá avaliar como as técnicas de APR se comportam em cenários mais complexos e práticos. Além disso, o projeto integrará o uso de Large Language Models (LLMs) para gerar, de forma automatizada, critérios de aceitação baseados nas especificações e erros identificados no código. Essa abordagem visa não apenas orientar o processo de reparo automático, mas também fornecer um conjunto de requisitos de teste robustos que facilitem a validação das correções aplicadas.

Outro ponto importante que será explorados inclui uma pipeline iterativa de reparo: a ideia é implementar um ciclo contínuo no qual a LLM gere inicialmente os critérios de aceitação e/ou sugestões de patch, modernize o sistema com base nos critérios gerados e quaisquer erros encontrados no novo código sejam realimentados para a LLM. Essa abordagem iterativa permite a evolução gradual do código, aprimorando tanto a funcionalidade quanto a legibilidade e manutenibilidade.

# Organização de pastas

- Formatted code: o código do sistema no formato enviado para a LLM; aquivos de compilação, configuração de IDE, e arquivos vazios, por exemplo, foram ignorados.
- Pastas separadas por modelo: organizar logs de conversas para testes e geração de código, segmentados em testes com descrições em linguagem natural.

# Motivação

A modernização de sistemas legados são um _wicked problem__, um desafio de difícil especificação que possui um número ilimitado de estratégias de solução, tornando-se um problema persistente dentro da engenharia de software [1]. Sistemas legados estão presentes em muitas indústrias, a exemplo de instituições governamentais nos Estados Unidos, que possui sistemas legados que foram escritos até mesmo 60 anos atrás. Esses sistemas desatualizados podem causar sérios riscos à eficiência, segurança, e manutenção de código dessas instituições.
 
Traditional approaches often rely on rule-based refactoring, static pattern matching, or complete system rewrites, all of which are costly, labor-intensive, and prone to introducing regressions. Moreover, developers and decision-makers remain cautious about fully integrating automation into modernization workflows, particularly for safety-critical systems where even minor unintended changes can introduce significant risks.

Large Language Models (LLMs) offer a promising new approach by not only assisting in code transformation but also in the generation of acceptance criteria—a crucial aspect of ensuring correctness. By leveraging LLMs to infer specifications from existing code, logs, or partial documentation, we can create robust test suites that serve as a foundation for both validation and automated repair. In turn, this enables a more structured, iterative modernization process that enhances reliability while reducing manual effort. Thus, this project explores the intersection of Automated Program Repair (APR) and LLM-generated acceptance criteria.

# Study Setting
Given the complexity and heterogeneity of real-world legacy systems, our experimental setup was designed to balance practical relevance with controlled evaluation. To this end, we selected a set of representative legacy projects written in older versions of Java, where shared coding patterns and constraints provide a realistic testbed for assessing automated repair techniques guided by LLM-generated acceptance criteria.

## Legacy Projects Selection

To simulate real-world conditions, we chose two projects from different application domains and with varying levels of complexity:

### Hipparcos-plot
A series of the European Spacial Agence tools written originally in 1997 and revisited, later, 9 years ago. The plot module of the tools were chosen to be modernized: it is responsible for generating graphs based on input data and it implements POO concepts.
### F-sharp
A music library management application that integrates with a native MySQL database. Written 16 years ago, this project exemplifies the challenges of legacy systems in managing database connections and adapting to new user requirements.

The choice of these projects is based on the premise that, even if they represent lower complexity compared to large-scale corporate systems, the inherent difficulties of legacy systems—such as outdated documentation, technical debt, and tightly coupled code—are pervasive and can be effectively exploited to validate the proposed techniques. Although the chosen projects have reduced complexity compared to large-scale legacy systems, their origin as legacy systems written in older Java versions offers a controlled yet realistic environment. The similarities among these systems (e.g., coding patterns, architectural constraints) allow us to extrapolate findings to more complex legacy scenarios.

## Research Questions

Our study is driven by the following core questions:

### RQ1
How effective are LLMs at generating acceptance criteria for code repair?
We assess the ability of LLMs to synthesize robust test cases and criteria that validate whether an automated repair maintains the intended behavior of legacy code.
### RQ2
How well do the APR capabilities of LLMs work on legacy code?
This question evaluates how well LLM-guided APR methods can handle the nuances of legacy systems, especially in the presence of convoluted logic and outdated coding practices.
### RQ3
How do the tests affect the repair process?
We investigate whether the presence of LLM-generated test suites improves repair outcomes compared to traditional prompt engineering techniques, analyzing if additional test cases guide the LLM to generate more accurate and reliable patches.

## LLM Models and Experimental Setup

For the automated generation of acceptance criteria and repair suggestions, we selected three state-of-the-art LLMs, each with distinct strengths in reasoning and code synthesis:

### GPT-3-o (Model 1)
Chosen for its advanced language understanding and coding capabilities, serving as a baseline for reasoning-intensive tasks.
### Deepseek R1 (Model 2)
Selected for its focus on code synthesis, this model is evaluated for its ability to identify patterns and suggest repairs in complex legacy code contexts.
### Gemini Flash Thinking (Model 3)
A model emphasizing both reasoning and code generation.

All of the above models were used with the default parameters.

# Methodology
Our methodology is designed to integrate LLM-generated acceptance criteria with an iterative automated repair process for legacy systems. The approach is structured into distinct phases: generation of acceptance criteria, utilization of LLMs for repair, and iterative validation and refinement of the generated patches.

## Generation of Acceptance Criteria

We generate two types of acceptance criteria for each legacy system:

### Natural Language Descriptions
The LLM is prompted to analyze the provided code—taking into account its purpose, inputs, and expected outputs—and generate a detailed narrative describing all necessary scenarios that verify the code’s correct behavior. This narrative acts as a set of acceptance criteria covering edge cases, functional requirements, and potential failure modes. The prompt for this task is:
"Analyze the code below, including its purpose, inputs, and expected outputs. Then, generate a detailed natural language description of all the necessary scenarios that verify whether the code functions correctly and meets its intended requirements. Consider that the code will be transformed for the Java 17 language."

### Traditional Unit and Integration Tests:
The LLM is also tasked with creating comprehensive test suites. The prompt for this task is:
"Analyze the code below, including its purpose, inputs, and expected outputs. Then, create a comprehensive test suite consisting of unit and/or integration tests that fully cover all aspects of the code's functionality, ensuring it produces a valid solution. Consider that the code will be transformed for the Java 17 language."

## LLM-Based Automated Repair Approaches

Once the acceptance criteria are generated, they are used to guide the repair process. We test several approaches to determine the most effective strategy:

### Test Suite Only Approach
The prompt includes only the generated test suites. This tests the LLM's ability to repair code based solely on dynamic behavioral specifications.
### Natural Language Description Only Approach
Only the natural language acceptance criteria are provided. This examines whether descriptive requirements can sufficiently guide the repair process without explicit test cases.
### Baseline Modernization Prompt
As a control, the LLM is given a simpler prompt such as "Modernize the code to Java version 17" without any additional acceptance criteria. This approach serves to benchmark the effectiveness of acceptance criteria in driving correct repairs.

For each legacy system file, the chosen LLMs (GPT-3-o, Deepseek R1, and Gemini Flash Thinking) are engaged with these various prompts, ensuring that the repair process is evaluated across different input strategies.

## Iterative Repair and Validation Process

The repair process is executed in an iterative cycle that comprises the following steps:

### Candidate Generation:
Based on the provided prompt and acceptance criteria, the LLM generates a modernized version of the legacy code.

###  Testing and Validation:
An attempt is made to compile and execute the code.

### Feedback and Refinement:
In cases where a candidate patch fails to be executed, error messages and test failures are fed back into the LLM. A new candidate is generated and testing is repeated.

## Evaluation Metrics and Reproducibility

To measure the effectiveness of our methodology, we consider multiple evaluation dimensions:

### Acceptance Criteria Quality
Evaluated by the comprehensiveness of the generated test suites and natural language descriptions, including coverage of edge cases and functional requirements.

### Modernization Quality
Evaluated by the easiness of integration, success/failure after compilation, and correct execution.

All prompts, test suites, and candidate patches are systematically organized in a dedicated directory structure (e.g., the “gemini” folder for conversation logs and test generation) to ensure full reproducibility and traceability of the experiments.For a more comprehensive read of our analysis, refer to the slidesheet presentation.


