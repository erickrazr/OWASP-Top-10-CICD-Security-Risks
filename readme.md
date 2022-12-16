
# Versão traduzida do Top 10 CI/CD Security Risks
# Idioma: PORTUGUÊS - BRASIL
<br />

![alt_text](assets/images/risks.png)
# Introdução

CI/CD environments, processes, and systems are the beating heart of any modern software organization. They deliver code from an engineer’s workstation to production. Combined with the rise of the DevOps discipline and microservice architectures, CI/CD systems and processes have reshaped the engineering ecosystem:

* The technical stack is more diverse, both in relation to coding languages as well as to technologies and frameworks adopted further down the pipeline (e.g. GitOps, K8s).
* Adoption of new languages and frameworks is increasingly quicker, without significant technical barriers.
* There is an increased use of automation and Infrastructure as Code (IaC) practices.
* 3rd parties, both in the shape of external providers as well as dependencies in code, have become a major part of any CI/CD ecosystem, with the integration of a new service typically requiring no more than adding 1-2 lines of code.

These characteristics allow faster, more flexible and diverse software delivery. However, they have also reshaped the attack surface with a multitude of new avenues and opportunities for attackers.

Adversaries of all levels of sophistication are shifting their attention to CI/CD, realizing CI/CD services provide an efficient path to reaching an organization’s crown jewels. The industry is witnessing a significant rise in the amount, frequency and magnitude of incidents and attack vectors focusing on abusing flaws in the CI/CD ecosystem, including - 

* The compromise of the **SolarWinds** build system, used to spread malware through to 18,000 customers.
* The **Codecov** breach, that led to exfiltration of secrets stored within environment variables in thousands of build pipelines across numerous enterprises.
* The **PHP breach**, resulting in publication of a malicious version of PHP containing a backdoor.
* The **Dependency Confusion** flaw, which affected dozens of giant enterprises, and abuses flaws in the way external dependencies are fetched to run malicious code on developer workstations and build environments.
* The compromises of the **_ua-parser-js_, _coa_ and _rc_ NPM packages**, with millions of weekly downloads each, resulting in malicious code running on millions of build environments and developer workstations.

While attackers have adapted their techniques to the new realities of CI/CD, most defenders are still early on in their efforts to find the right ways to detect, understand, and manage the risks associated with these environments. Seeking the right balance between optimal security and engineering velocity, security teams are in search for the most effective security controls that will allow engineering to remain agile without compromising on security.

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

[CICD-SEC-6](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-06-Insufficient-Credential-Hygiene-PT-BR.md): Insufficient Credential Hygiene

[CICD-SEC-7](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-07-Configuracoes-Inseguras-de-Sistemas-PT-BR.md): Configurações Inseguras de Sistemas

[CICD-SEC-8](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-08-Uso-nao-controlado-de-servicos-de-terceiros-PT-BR.md): Uso não controlado de serviços de terceiros

[CICD-SEC-9](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-09-Validacao-de-integridade-de-artefatos-improprias-PT-BR.md): Validação de integridade de artefatos impróprias

[CICD-SEC-10](https://github.com/erickrazr/www-project-top-10-ci-cd-security-risks/blob/main/CICD-SEC-10-Visibilidade-e-Logs-Insuficientes-PT-BR.md):Visibilidade e Logs Insuficientes


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

