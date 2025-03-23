# Legacy Code Repair Using LMM Generated Acceptance Criteria

O projeto propõe reavaliar e expandir os experimentos apresentados no estudo “Automated Repair of Programs from Large Language Models”, introduzindo novas perspectivas e funcionalidades que visam aumentar a aplicabilidade e a eficácia das técnicas de reparo automático de código. Em vez de focar apenas em problemas competitivos, como os do LeetCode, a ideia central é concentrar os esforços em sistemas legados, onde a manutenção e evolução do código são desafios reais em ambientes corporativos.

Para isso, serão reproduzidos os experimentos originais, adaptando-os para o contexto de código legado, o que permitirá avaliar como as técnicas de APR se comportam em cenários mais complexos e práticos. Além disso, o projeto integrará o uso de Large Language Models (LLMs) para gerar, de forma automatizada, critérios de aceitação baseados nas especificações e erros identificados no código. Essa abordagem visa não apenas orientar o processo de reparo automático, mas também fornecer um conjunto de requisitos de teste robustos que facilitem a validação das correções aplicadas.

Outros pontos importantes que serão explorados incluem:

• Diversificação do Conjunto de Dados:
Ampliar a análise para incluir projetos open source e sistemas legados de diversos domínios (por exemplo, financeiro, saúde, etc.), permitindo uma compreensão mais ampla dos defeitos comuns e dos desafios de manutenção.

• Integração com Ferramentas de Análise Estática:
Utilizar técnicas de análise estática para identificar padrões recorrentes de defeitos e “code smells” em sistemas legados, os quais poderão orientar a geração de critérios de aceitação e a aplicação de correções mais precisas.

• Pipeline Iterativo de Reparo:
Implementar um ciclo contínuo no qual a LLM gere inicialmente os critérios de aceitação e/ou sugestões de patch, que serão validadas e refinadas por meio de técnicas de APR. Essa abordagem iterativa permite a evolução gradual do código, aprimorando tanto a funcionalidade quanto a legibilidade e manutenibilidade.

• Abordagem Híbrida:
Combinar os pontos fortes dos métodos tradicionais de APR com as capacidades de síntese e edição de código dos LLMs. Isso pode incluir, por exemplo, a extração de “ingredientes” de patches a partir de múltiplas soluções geradas pela LLM, possibilitando a criação de correções mais robustas e complexas.

# Organização de pastas

- Formatted code: o código do sistema no formato enviado para a LLM; aquivos de compilação, configuração de IDE, e arquivos vazios, por exemplo, foram ignorados.
- gemini: organizar logs de conversas para testes e geração de código.

# Motivation

Legacy systems are a persistent challenge in software engineering, often burdened with security vulnerabilities, technical debt, and high maintenance costs. In the U.S. federal government alone, many legacy systems range from 30 to over 60 years old [1], creating substantial obstacles related to efficiency, maintainability, staffing, and security. These issues are not confined to government institutions—many industries, including finance, healthcare, and manufacturing, rely on outdated software that is difficult to adapt to modern requirements.

The modernization of legacy software has been described as a “wicked problem” [2], [3], resisting decades of well-intentioned policies and technical initiatives. Traditional approaches often rely on rule-based refactoring, static pattern matching, or complete system rewrites, all of which are costly, labor-intensive, and prone to introducing regressions. Moreover, developers and decision-makers remain cautious about fully integrating automation into modernization workflows, particularly for safety-critical systems where even minor unintended changes can introduce significant risks.

A key barrier to modernization is validation. Since many legacy systems lack comprehensive documentation and test suites, verifying that modernized code preserves the original behavior is a major challenge. Without a well-defined correctness criterion, automated repair techniques can introduce subtle errors or fail to make meaningful improvements.

Large Language Models (LLMs) offer a promising new approach by not only assisting in code transformation but also in the generation of acceptance criteria—a crucial aspect of ensuring correctness. By leveraging LLMs to infer specifications from existing code, logs, or partial documentation, we can create robust test suites that serve as a foundation for both validation and automated repair. In turn, this enables a more structured, iterative modernization process that enhances reliability while reducing manual effort.

This project explores the intersection of Automated Program Repair (APR) and LLM-generated acceptance criteria, adapting techniques initially tested on competitive programming problems to real-world legacy systems. Our goal is to bridge the gap between theoretical APR advancements and practical modernization challenges, ensuring that automated fixes are trustworthy, scalable, and aligned with domain-specific constraints.

# Methodology
Our methodology is designed to integrate LLM-generated acceptance criteria with an iterative automated repair process for legacy systems. The approach is structured into distinct phases: generation of acceptance criteria, utilization of LLMs for repair, and iterative validation and refinement of the generated patches.

## Generation of Acceptance Criteria

We generate two types of acceptance criteria for each legacy system:

### Natural Language Descriptions
The LLM is prompted to analyze the provided code—taking into account its purpose, inputs, and expected outputs—and generate a detailed narrative describing all necessary scenarios that verify the code’s correct behavior. This narrative acts as a set of acceptance criteria covering edge cases, functional requirements, and potential failure modes.
### Traditional Unit and Integration Tests:
The LLM is also tasked with creating comprehensive test suites. The prompt for this task is:
"Analyze the code below, including its purpose, inputs, and expected outputs. Then, create a comprehensive test suite consisting of unit and/or integration tests that fully cover all aspects of the code's functionality, ensuring it produces a valid solution. Consider that the code will be transformed for the Java 17 language."

In cases where the system code is too lengthy to include in a single message, all files are segmented and processed iteratively to ensure complete context is provided to the LLM.

## LLM-Based Automated Repair Approaches

Once the acceptance criteria are generated, they are used to guide the repair process. We test several approaches to determine the most effective strategy:

### Combined Input Approach
Both the natural language descriptions and the generated test suites are inserted as part of the prompt. This enriched context helps the LLM understand the functional requirements and expected behavior in greater depth.
### Test Suite Only Approach
The prompt includes only the generated test suites. This tests the LLM's ability to repair code based solely on dynamic behavioral specifications.
### Natural Language Description Only Approach
Only the natural language acceptance criteria are provided. This examines whether descriptive requirements can sufficiently guide the repair process without explicit test cases.
### Baseline Modernization Prompt
As a control, the LLM is given a simpler prompt such as "Modernize the code to Java version 17" without any additional acceptance criteria. This approach serves to benchmark the effectiveness of acceptance criteria in driving correct repairs.

For each legacy system file, the chosen LLMs (GPT-4-o, Deepseek R1, and Gemini Flash Thinking) are engaged with these various prompts, ensuring that the repair process is evaluated across different input strategies.

## Iterative Repair and Validation Process

The repair process is executed in an iterative cycle that comprises the following steps:

### Candidate Generation:
Based on the provided prompt and acceptance criteria, the LLM generates one or more candidate patches for the legacy code.
### Automated Testing and Validation:
The generated candidate patches are run against the test suites (both the LLM-generated tests and any pre-existing regression tests). A patch is considered successful if it passes all tests and meets the criteria outlined in the natural language description.
### Feedback and Refinement:
In cases where a candidate patch fails to meet the acceptance criteria, error messages and test failures are fed back into the system. The prompt is then refined—either manually or via an automated feedback loop—and the LLM is prompted again to generate improved repair suggestions.

## Integration of Static Analysis
Before submitting code to the LLM, static analysis tools are applied to each legacy system. These tools identify code smells, common defect patterns, and structural issues. The insights from static analysis can optionally be appended to the prompts to provide additional context, further guiding the LLM in both acceptance criteria generation and repair.

## Evaluation Metrics and Reproducibility

To measure the effectiveness of our methodology, we consider multiple evaluation dimensions:

### Acceptance Criteria Quality
Evaluated by the comprehensiveness of the generated test suites and natural language descriptions, including coverage of edge cases and functional requirements.
### Repair Success Rate
Measured by the proportion of candidate patches that pass all tests and are validated as correct.
### Iterative Improvement
Assessed by tracking improvements in candidate patch quality across iterations, based on feedback from failed tests.
### Comparative Analysis
We compare the performance of different input strategies (combined, tests-only, natural language-only, and baseline) across multiple legacy systems to determine which approach yields the most robust repairs.

All prompts, test suites, candidate patches, and evaluation logs are systematically organized in a dedicated directory structure (e.g., the “gemini” folder for conversation logs and test generation) to ensure full reproducibility and traceability of the experiments.


