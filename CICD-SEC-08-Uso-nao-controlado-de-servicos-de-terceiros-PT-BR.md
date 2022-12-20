
## Definição

A superfície de ataque CI/CD consiste nos ativos orgânicos de uma organização, como SCM ou CI, e nos serviços de terceiros que recebem acesso a esses ativos orgânicos. Os riscos relacionados ao uso não controlado de serviços de terceiros dependem da extrema facilidade com que um serviço de terceiros pode obter acesso a recursos em sistemas CI/CD, expandindo efetivamente a superfície de ataque da organização.

## Descrição

É raro encontrar uma organização que não tenha vários terceiros conectados aos seus sistemas e processos de CI/CD. A sua facilidade de implementação, aliada ao seu valor imediato, tornou os terceiros parte integrante do dia-a-dia do desenvolvimento. Os métodos de incorporação ou concessão de acesso a terceiros estão se tornando mais diversificados e as complexidades associadas à sua implementação estão diminuindo.

Tomando um SCM comum - GitHub SAAS - como exemplo, aplicações de terceiros podem ser conectados por meio de um ou mais destes 5 métodos:



* GitHub Application
* OAuth application
* Fornecimento de um token de acesso fornecido a aplicações de terceiros.
* Fornecimento de uma chave SSH fornecida a aplicações de terceiros.
* Configurar eventos de webhook para serem enviados para terceiros.

Cada método leva entre segundos e minutos para ser implementado e concede a terceiros vários recursos, desde a leitura de código em um único repositório até a administração completa da organização GitHub. Apesar do nível potencialmente alto de permissão que esses terceiros recebem ao sistema, em muitos casos nenhuma permissão ou aprovação especial é exigida pela organização antes da implementação real.

Os sistemas de build também permitem fácil integração a terceiros. A integração de terceiros em pipelines de compilação geralmente não é mais complexa do que adicionar 1-2 linhas de código no arquivo de configuração do pipeline ou instalar um plug-in de mercado do sistema de build (por exemplo, ações no Github Actions, Orbs no CircleCI). A funcionalidade de terceiros é então importada e executada como parte do processo do build com acesso total a quaisquer recursos disponíveis no estágio de pipeline em que é executado.

Métodos semelhantes de conectividade estão disponíveis em vários formatos e formas na maioria dos sistemas CI/CD, tornando extremamente complexo o processo de gerir e manter privilégios mínimos em relação ao uso de terceiros em todo o ecossistema de desenvolvimento. As organizações estão enfrentando o desafio de obter total visibilidade sobre quais terceiros têm acesso aos diferentes sistemas, quais métodos de acesso eles têm, que nível de permissão/acesso receberam e que nível de permissões/acesso estão realmente usando.

## Impacto

A falta de governança e visibilidade em relação às implementações de terceiros impede que as organizações mantenham o RBAC em seus sistemas de CI/CD. Dado o quão permissivos terceiros tendem a ser, as organizações são tão seguras quanto os terceiros que implementam. Implementação insuficiente de RBAC e menos privilégio em relação a terceiros, juntamente com governança e diligência mínimas em torno do processo de implementações de terceiros, criam um aumento significativo da superfície de ataque da organização.

Dada a natureza altamente interconectada dos sistemas e ambientes de CI/CD, o comprometimento de um único terceiro pode ser aproveitado para causar danos muito além do escopo do sistema ao qual o terceiro está conectado (por exemplo, um terceiro com permissões de escrita em um repositório, pode ser aproveitado por um adversário para enviar código para o repositório que, por sua vez, acionará uma compilação e executará o código malicioso do adversário no sistema de compilação).


## Recomendações

Os controles de governança em torno dos serviços de terceiros devem ser implementados em cada estágio do ciclo de vida de uso destes:



* **Aprovação** - Estabeleça procedimentos de verificação para garantir que terceiros com acesso a recursos em qualquer lugar do ecossistema de engenharia sejam aprovados antes de receberem acesso ao ambiente e que o nível de permissão concedido esteja alinhado com o princípio de mínimo privilégio.
* **Integração** - Introduzir controles e procedimentos para manter a visibilidade contínua de todos os terceiros integrados aos sistemas CI/CD, incluindo:
     * Método de integração. Certifique-se de que todos os métodos de integração para cada sistema sejam cobertos (incluindo aplicações de mercado, plug-ins, aplicativos OAuth, tokens de acesso programático etc.).
     * Nível de permissão concedido a terceiros.
     * Nível de permissão atualmente em uso pelo terceiro.
* **Visibilidade sobre o uso contínuo** - Certifique-se de que cada terceiro esteja limitado e com escopo para os recursos específicos aos quais requer acesso e remova permissões não utilizadas e/ou redundantes. Terceiros que são integrados como parte do processo de build devem ser executados dentro de um contexto com escopo com acesso limitado a segredos e código e com filtros rígidos de entrada e saída.
* **Desprovisionamento** - Revise periodicamente todos os terceiros integrados e remova aqueles que não estão mais em uso.


## Referências



1. Codecov, a popular code coverage tool used in the CI, is compromised to steal environment variables from builds.

    [https://about.codecov.io/security-update/](https://about.codecov.io/security-update/)

2. Attackers compromise a GitHub user account of a DeepSource (a static analysis platform) engineer. Using the compromised account, they obtain the permissions of the DeepSource GitHub application, granting them full access to the codebase of all DeepSource clients that have installed the compromised GitHub application. 

    [https://discuss.deepsource.io/t/security-incident-on-deepsource-s-github-application/131](https://discuss.deepsource.io/t/security-incident-on-deepsource-s-github-application/131)

3. Attackers gain access to the database of Waydev, a git analytics platform, stealing GitHub and GitLab OAuth tokens of their customers.

    [https://changelog.waydev.co/github-and-gitlab-oauth-security-update-dw98s](https://changelog.waydev.co/github-and-gitlab-oauth-security-update-dw98s)
