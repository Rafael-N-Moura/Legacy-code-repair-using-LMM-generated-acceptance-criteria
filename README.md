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
