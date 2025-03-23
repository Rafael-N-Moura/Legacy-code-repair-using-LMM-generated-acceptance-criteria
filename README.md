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
