
# Versão traduzida do Top 10 CI/CD Security Risks
# Idioma: PORTUGUÊS - BRASIL
## Traduzido por: Erick Ferreira 
## Contato: https://www.linkedin.com/in/erickrazr/
<br />

![alt_text](assets/images/risks.png)
# Introdução

Ambientes, processos e sistemas de CI/CD são o coração pulsante de qualquer organização de software moderna. Eles entregam o código da estação de trabalho de um desenvolvedor para a produção. Combinados com a ascensão da disciplina DevOps e das arquiteturas de microsserviços, os sistemas e processos de CI/CD remodelaram o ecossistema de desenvolvimento:

* O Conjunto de tecnologias é mais diversificado, tanto em relação às linguagens de codificação quanto às tecnologias e estruturas adotadas posteriormente (por exemplo, GitOps, K8s).
* A adoção de novas linguagens e frameworks está cada vez mais rápida, sem grandes barreiras técnicas.
* Há um aumento no uso de práticas de automação e infraestrutura como código (IaC).
* Terceiros, tanto na forma de provedores externos quanto de dependências no código, tornaram-se uma parte importante de qualquer ecossistema de CI/CD, com a integração de um novo serviço normalmente exigindo não mais do que adicionar 1 a 2 linhas de código.

Essas características permitem uma entrega de software mais rápida, flexível e diversificada. No entanto, eles também reformularam a superfície de ataque com uma infinidade de novos caminhos e oportunidades para os atacantes.

Adversários de todos os níveis de sofisticação estão voltando sua atenção para CI/CD, percebendo que os serviços de CI/CD fornecem um caminho eficiente para alcançar as joias da coroa de uma organização. A indústria está testemunhando um aumento significativo na quantidade, frequência e magnitude de incidentes e vetores de ataque com foco no abuso de falhas no ecossistema CI/CD, incluindo -

* O comprometimento do sistema de compilação **SolarWinds**, usado para espalhar malware para 18.000 clientes.
* A violação **Codecov**, que levou à exfiltração de segredos armazenados em variáveis de ambiente em milhares de pipelines de build em várias empresas.
* A **violação do PHP**, resultando na publicação de uma versão maliciosa do PHP contendo um backdoor.
* A falha **Dependency Confusion**, que afetou dezenas de empresas gigantes, e abusa de falhas na forma como dependências externas são buscadas para executar código malicioso em estações de trabalho de desenvolvedores e ambientes de build.
* Os compromentimentos dos pacotes **_ua-parser-js_, _coa_ e _rc_ NPM**, com milhões de downloads semanais cada, resultando em código malicioso executado em milhões de ambientes de construção e estações de trabalho de desenvolvedores.

Embora os invasores tenham adaptado suas técnicas às novas realidades de CI/CD, a maioria dos defensores ainda está no início de seus esforços para encontrar as maneiras corretas de detectar, entender e gerenciar os riscos associados a esses ambientes. Buscando o equilíbrio certo entre segurança ideal e velocidade de desenvolvimento, as equipes de segurança estão em busca dos controles mais eficazes que permitirão que o desenvolvimento permaneça ágil sem comprometer a segurança.

# Top 10 risks
Todos os riscos seguem uma estrutura consistente:

* **Definição** - Definição concisa da natureza do risco.
* **Descrição** - Explicação detalhada do contexto e motivação do adversário.
* **Impacto** - Detalhe sobre o impacto potencial que a realização do risco pode ter em uma organização.
* **Recomendações** - Conjunto de medidas e controles recomendados para otimizar a postura de CI/CD de uma organização em relação ao risco em questão.
* **Referências** - Uma lista de exemplos e precedentes do mundo real em que o risco em questão foi explorado.

A lista foi compilada com base em extensa pesquisa e análise com base nas seguintes fontes:

* Análise da postura de arquitetura, design e segurança de centenas de ambientes de CI/CD em vários setores e setores.
* Discussões profundas com especialistas do setor.
* Publicações detalhando incidentes e falhas de segurança dentro do domínio de segurança CI/CD. Exemplos são fornecidos quando relevantes.


# Lista dos Top 10 Riscos de Segurança em CI/CD:

[CICD-SEC-1](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-01-Mecanismos-de-Controle-de-Fluxo-Insuficiente-PT-BR.md): Mecanismos de Controle de Fluxo Insuficientes

[CICD-SEC-2](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-02-Gerenciamento-de-Identidade-e-Acessos-inadequado-PT-BR.md): Gerenciamento de Identidade e Acessos inadequado

[CICD-SEC-3](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-03-Abuso-de-cadeia-de-dependencia-PT-BR.md): Abuso de cadeia de dependência

[CICD-SEC-4](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-04-Poisoned-Pipeline-Execution-PT-BR.md): Execução de Pipeline Envenendada - *Poisoned Pipeline Execution (PPE)*

[CICD-SEC-5](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-05-PBAC-Insuficiente-PT-BR.md): Controle de Acesso Insuficiente em Pipelines - *Insufficient PBAC (Pipeline-Based Access Controls)*

[CICD-SEC-6](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-06-Gestao-de-Credenciais-Inadequadas-PT-BR.md): Gestão de Credenciais Inadequada

[CICD-SEC-7](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-07-Configuracoes-Inseguras-de-Sistemas-PT-BR.md): Configurações Inseguras de Sistemas

[CICD-SEC-8](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-08-Uso-nao-controlado-de-servicos-de-terceiros-PT-BR.md): Uso não controlado de serviços de terceiros

[CICD-SEC-9](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-09-Validacao-de-integridade-de-artefatos-improprias-PT-BR.md): Validação de integridade de artefatos impróprias

[CICD-SEC-10](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-10-Visibilidade-e-Logs-Insuficientes-PT-BR.md): Visibilidade e Logs Insuficientes


#  A Iniciativa "Top 10 CI/CD Security Risks" 

Este documento ajuda os defensores a identificar áreas de foco para proteger seu ecossistema de CI/CD. É o resultado de uma extensa pesquisa sobre vetores de ataque associados a CI/CD e da análise de violações de alto perfil e falhas de segurança.

Inúmeros especialistas da área em vários setores e disciplinas se reuniram para colaborar neste documento para garantir sua relevância para o cenário atual de ameaças, superfície de risco e os desafios que os defensores enfrentam ao lidar com esses riscos.

Gostaríamos de agradecer e reconhecer todos os especialistas que participaram da revisão e validação deste documento..


## Autores

* [Daniel Krivelevich](https://twitter.com/Dkrivelev) (CTO @ Cider Security)
* [Omer Gil](https://twitter.com/omer_gil) (Director of Research @ Cider Security)


## Revisores

* [Iftach Ian Amit](https://twitter.com/iiamit) (Advisory CSO @ Rapid7)
* [Jonathan Claudius](https://twitter.com/claudijd) (CISO @ Jump Crypto)
* [Michael Coates](https://twitter.com/_mwc) (CEO & Co-Founder @ Altitude Networks, Former CISO @ Twitter)
* Jonathan Jaffe (CISO @ Lemonade Insurance)
* Adrian Ludwig (Chief Trust Officer @ Atlassian)
* [Travis McPeak](https://twitter.com/travismcpeak) (Head of Product Security @ Databricks)
* Ron Peled (Founder & CEO @ ProtectOps, Former CISO @ LivePerson)
* [Ty Sbano](https://twitter.com/tysbano) (CISO @ Vercel)
* [Astha Singhal](https://twitter.com/astha_singhal) (Director of Application Security @ Netflix)
* [Hiroki Suezawa](https://twitter.com/rung) (Security Engineer @ Mercari, inc.)
* Tyler Welton (Principal Security Engineer @ Built Technologies, Owner @ Untamed Theory)
* Tyler Young (Head of Security at Relativity)
* [Ory Segal](https://twitter.com/orysegal) (Senior Director, Product Management @ Palo Alto Networks)
* Noa Ginzbursky (DevOps Engineer @ Cider Security)
* [Asi Greenholts](https://twitter.com/TupleType) (Security Researcher @ Cider Security)


## Disclaimer

O texto original é a versão em inglês do site: https://github.com/OWASP/www-project-top-10-ci-cd-security-risks . Quaisquer discrepâncias ou diferenças criadas na tradução não são vinculativas e não têm efeito legal para fins de conformidade.

A precisão do texto traduzido não é garantida. Qualquer pessoa que confie nas informações obtidas através desta tradução o faz por sua própria conta e risco. O @erickrazr se isenta e não aceitará qualquer responsabilidade por danos ou perdas de qualquer tipo causados pelo uso da documentação traduzida. Onde houver alguma dúvida, a versão em inglês é sempre a versão oficial do site: https://github.com/OWASP/www-project-top-10-ci-cd-security-risks
