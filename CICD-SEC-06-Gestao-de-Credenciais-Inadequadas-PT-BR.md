
## Definição


Os riscos de gestão de credenciais inadequadas lidam com a capacidade de um invasor obter e usar vários segredos e tokens espalhados pelo pipeline devido a falhas relacionadas a controles de acesso em torno das credenciais, gerenciamento inseguro de segredos e credenciais excessivamente permissivas.


## Descrição

Os ambientes de CI/CD são construídos com vários sistemas se comunicando e autenticando entre si, criando grandes desafios em relação à proteção de credenciais devido à grande variedade de contextos em que as credenciais podem existir.

Credenciais de aplicações são usadas pelo aplicação em tempo de execução, credenciais para sistemas de produção são usadas por pipelines para implantar infraestrutura, artefatos e aplicativos para produção, desenvolvedores usam credenciais como parte de seus ambientes de teste e dentro de seus códigos e artefatos.

Essa variedade de contextos, aliada à grande quantidade de métodos e técnicas para armazená-los e utilizá-los, cria um grande potencial para uso inseguro de credenciais. Algumas falhas importantes que afetam a gestão da credencial:



* **Código contendo credenciais sendo enviado para um dos branches de um repositório SCM:** Isso pode ser por engano - sem perceber a existência do segredo no código, ou deliberadamente - sem entender o risco de fazer isso. A partir desse momento, as credenciais são expostas a qualquer pessoa com acesso de leitura ao repositório e, mesmo que sejam excluídas da ramificação para a qual foram enviadas, elas continuam aparecendo no histórico de commits, disponíveis para visualização por qualquer pessoa com acesso ao repositório.
* **Credenciais usadas de forma insegura nos processos de construção e implantação:** essas credenciais são usadas para acessar repositórios de código, ler e gravar em repositórios de artefatos e implantar recursos e artefatos em ambientes de produção. Dada a grande quantidade de pipelines e sistemas aos quais eles precisam acessar, é imperativo entender -
     * Em que contexto e usando qual método cada conjunto de credenciais é usado?
     * Cada pipeline pode acessar apenas as credenciais necessárias para cumprir sua finalidade?
     * As credenciais podem ser acessadas por código não revisado que flui pelo pipeline?
     * Como essas credenciais são chamadas e injetadas no build? Essas credenciais são acessíveis apenas em tempo de execução e apenas nos contextos em que são necessárias?
* **Credenciais nos layers da imagem do contêiner:** As credenciais que eram necessárias apenas para criar a imagem ainda existem em um dos layers da imagem - disponíveis para qualquer pessoa que possa fazer o download da imagem.
* **Credenciais exibidas nos logs:** As credenciais usadas em pipelines geralmente são exibidas na saída do log, deliberadamente ou inadvertidamente. Isso pode deixar as credenciais expostas em texto não criptografado nos logs, disponíveis para visualização por qualquer pessoa com acesso aos resultados do build. Esses logs podem potencialmente fluir para sistemas de gerenciamento de logs, expandindo sua superfície de exposição.
* **Credenciais não rotativas:** como as credenciais estão espalhadas por todo o ecossistema de engenharia, elas são expostas a um grande número de funcionários e contratados. A falha na rotação de credenciais resulta em uma quantidade cada vez maior de pessoas e artefatos que possuem credenciais válidas. Isso é especialmente verdadeiro para credenciais usadas por pipelines - por exemplo, chaves de implantação - que geralmente são gerenciadas usando a diretiva "Se não estiver quebrado, não conserte" - o que deixa as credenciais válidas sem rotação por muitos anos.


## Impacto

As credenciais são os objetos mais procurados pelos atacantes, que buscam usá-las para acessar recursos de alto valor ou para implantar códigos e artefatos maliciosos. Nesse contexto, os ambientes de desenvolvimento fornecem aos invasores vários caminhos para obter credenciais. O grande potencial de erro humano, juntamente com as lacunas de conhecimento em torno do gerenciamento seguro de credenciais e a preocupação de quebrar processos devido à rotação de credenciais, colocam os recursos de alto valor de muitas organizações em risco de comprometimento devido à exposição de suas credenciais.


## Recomendação



* Estabeleça procedimentos para mapear continuamente as credenciais encontradas nos diferentes sistemas do ecossistema de desenvolvimento - do código à implantação. Certifique-se de que cada conjunto de credenciais siga o princípio do menor privilégio e tenha recebido o conjunto exato de permissões necessárias para o serviço que o utiliza.
* Evite compartilhar o mesmo conjunto de credenciais em vários contextos. Isso aumenta a dificuyldade de seguir o princípio do menor privilégio, além de ter um efeito negativo na responsabilidade.
* Prefira usar credenciais temporárias em vez de credenciais estáticas. Caso as credenciais estáticas precisem estar em uso - estabeleça um procedimento para alternar periodicamente todas as credenciais estáticas e detectar credenciais obsoletas.
* Configure o uso de credenciais para ser limitado a condições predeterminadas (como escopo para um IP ou identidade de origem específica) para garantir que, mesmo em caso de comprometimento, as credenciais exfiltradas não possam ser usadas fora do seu ambiente.
* Detecte segredos enviados e armazenados em repositórios de código. Use controles como um plug-in IDE para identificar os segredos usados nas alterações locais, verificação automática a cada push de código e verificações periódicas no repositório e em suas confirmações anteriores.
* Certifique-se de que os segredos usados em sistemas de CI/CD tenham escopo definido de maneira a permitir que cada pipeline e etapa tenham acesso apenas aos segredos necessários.
* Use as opções internas do fornecedor ou ferramentas de terceiros para evitar que os segredos sejam impressos nas saídas do console de compilações futuras. Certifique-se de que todas as saídas existentes não contenham segredos.
* Verifique se os segredos foram removidos de qualquer tipo de artefato, como camadas de imagens de contêiner, binários ou gráficos do Helm.


## Referências



1. Thousands of credentials, stored as environment variables, were stolen by attackers through compromising Codecov, a popular code coverage tool used in the CI.

    [https://about.codecov.io/security-update/](https://about.codecov.io/security-update/)

2. Travis CI injected secure environment variables of public repositories into pull request builds, causing them to be susceptible to compromise by anonymous users issuing pull requests against public repositories. 

    [https://travis-ci.community/t/security-bulletin/12081](https://travis-ci.community/t/security-bulletin/12081)

3. An attacker compromised the TeamCity Build server of Stack Overflow and was able to steal secrets due to their insecure storage method.

    [https://stackoverflow.blog/2021/01/25/a-deeper-dive-into-our-may-2019-security-incident/](https://stackoverflow.blog/2021/01/25/a-deeper-dive-into-our-may-2019-security-incident/)

4. Samsung exposed overly permissive secrets in public GitLab repositories.

    [https://techcrunch.com/2019/05/08/samsung-source-code-leak/](https://techcrunch.com/2019/05/08/samsung-source-code-leak/)

5. Attackers accessed Uber’s private GitHub repositories that contained permissive and shared AWS tokens, leading to data exfiltration of millions of drivers and passengers. [https://www.ftc.gov/system/files/documents/federal_register_notices/2018/04/152_3054_uber_revised_consent_analysis_pub_frn.pdf](https://www.ftc.gov/system/files/documents/federal_register_notices/2018/04/152_3054_uber_revised_consent_analysis_pub_frn.pdf)
6. Gaining write access to Homebrew, by Eric Holmes. The Homebrew Jenkins instance revealed environment variables of executed builds, including a GitHub token which allowed an attacker to make malicious changes to the Homebrew project itself.

    [https://medium.com/@vesirin/how-i-gained-commit-access-to-homebrew-in-30-minutes-2ae314df03ab](https://medium.com/@vesirin/how-i-gained-commit-access-to-homebrew-in-30-minutes-2ae314df03ab)
