
## Definição


Os riscos de configuração insegura do sistema decorrem de falhas nas configurações de segurança dos diferentes sistemas em todo o pipeline (por exemplo, SCM, CI, repositório de artefatos), frequentemente resultando em "alvos fáceis" para invasores que desejam expandir sua superfície de ataque no ambiente.


## Descrição

Os ambientes de CI/CD são compostos por vários sistemas, fornecidos por várias empresas. Para otimizar a segurança do CI/CD, os times de segurança devem colocar forte ênfase no código, nos artefatos que fluem pelo pipeline e na postura e resiliência de cada sistema.

De maneira similar a outros sistemas que armazenam e processam dados, os sistemas CI/CD envolvem várias configurações de segurança em todos os níveis - aplicação, rede e infraestrutura. Essas configurações têm grande influência na postura de segurança dos ambientes de CI/CD e na suscetibilidade a um possível comprometimento. Adversários de todos os níveis de sofisticação estão sempre atentos a potenciais vulnerabilidades de CI/CD e configurações incorretas que podem ser aproveitadas em seu benefício.

Exemplos de possíveis falhas de harderning:



* Um sistema e/ou componente usando uma versão desatualizada ou sem patches de segurança importantes.
* Um sistema com controles de acesso à rede excessivamente permissivos.
* Um sistema que possui permissões administrativas no sistema operacional subjacente.
* Um sistema com configurações de sistema inseguras. As configurações geralmente determinam os principais recursos de segurança relacionados à autorização, controles de acesso, registro e muito mais. Em muitos casos, o conjunto padrão de configurações não é seguro e requer otimização.
* Um sistema com gestão de credencial inadequada - por exemplo, credenciais padrão que não são desativadas, tokens programáticos excessivamente permissivos e muito mais.

Embora o uso de soluções de CI/CD de Software como Serviço (SaaS), em vez de sua alternativa self-hosted, elimine alguns dos riscos potenciais associados ao hardening do sistema e ao movimento lateral dentro da rede, as organizações ainda precisam ser altamente diligentes na segurança, configurando sua solução SaaS CI/CD. Cada solução tem seu próprio conjunto de configurações de segurança exclusivas e práticas recomendadas essenciais para manter uma postura de segurança ideal.


## Impacto

Uma falha de segurança em um dos sistemas CI/CD pode ser aproveitada por um adversário para obter acesso não autorizado ao sistema ou pior - comprometer o sistema e acessar o sistema operacional subjacente. Essas falhas podem ser abusadas por um invasor para manipular fluxos legítimos de CI/CD, obter tokens confidenciais e potencialmente acessar ambientes de produção. Em alguns cenários, essas falhas podem permitir que um invasor se mova lateralmente dentro do ambiente e fora do contexto dos sistemas CI/CD.

## Recomendações



* Manter um inventário de sistemas e versões em uso, incluindo mapeamento de um proprietário designado para cada sistema. Verifique continuamente as vulnerabilidades conhecidas nesses componentes. Se um patch de segurança estiver disponível, atualize o componente vulnerável. Caso contrário, considere a remoção do componente/sistema ou reduza o impacto potencial da exploração da vulnerabilidade restringindo o acesso ao sistema ou a capacidade do sistema de executar operações confidenciais.
* Certifique-se de que o acesso à rede aos sistemas esteja alinhado com o princípio do menor privilégio.
* Estabeleça um processo para revisar periodicamente todas as configurações do sistema para qualquer configuração que possa afetar a postura de segurança do sistema e garantir que todas as configurações sejam ideais.
* Certifique-se de que as permissões para os nós de execução do pipeline sejam concedidas de acordo com o princípio do menor privilégio. Um erro de configuração comum nesse contexto é conceder permissões de debug em nós de execução para desenvolvedores. Embora em muitas organizações esta seja uma prática comum, é imperativo levar em consideração que qualquer usuário com a capacidade de acessar o nó de execução no modo de debug pode expor todos os segredos enquanto eles são carregados na memória e usam a identidade do nó - efetivamente concedendo permissões administrativas para qualquer desenvolvedor com este perfil.


## Referências



1. The compromise of the SolarWinds build system, used to spread malware through SolarWinds to 18,000 organizations.

	[https://sec.report/Document/0001628280-20-017451/#swi-20201214.htm](https://sec.report/Document/0001628280-20-017451/#swi-20201214.htm)



2. Backdoor planted in the PHP git repository. The attackers pushed malicious unreviewed code directly to the PHP main branch, ultimately resulting in a formal PHP version being spread to all PHP users. The attack presumably originated in a compromise of the PHP self-maintained git server.

    [https://news-web.php.net/php.internals/113981](https://news-web.php.net/php.internals/113981)

3. An attacker compromised Stack Overflow’s TeamCity build server, which was accessible from the internet.

    [https://stackoverflow.blog/2021/01/25/a-deeper-dive-into-our-may-2019-security-incident/](https://stackoverflow.blog/2021/01/25/a-deeper-dive-into-our-may-2019-security-incident/)

4. Attackers compromised an unpatched Webmin build server, and added a backdoor to the local copy of the code after being fetched from the repository, leading to a supply chain attack on servers using Webmin.

    [https://www.webmin.com/exploit.html](https://www.webmin.com/exploit.html)

5. Nissan source code leaked after a self-managed Bitbucket instance left accessible from the internet with default credentials.

    [https://www.zdnet.com/article/nissan-source-code-leaked-online-after-git-repo-misconfiguration/](https://www.zdnet.com/article/nissan-source-code-leaked-online-after-git-repo-misconfiguration/)

6. Mercedes Benz source code leaked after a self-maintained internet-facing GitLab server was made open for self-registration.

    [https://www.zdnet.com/article/mercedes-benz-onboard-logic-unit-olu-source-code-leaks-online/](https://www.zdnet.com/article/mercedes-benz-onboard-logic-unit-olu-source-code-leaks-online/)

7. A self-managed GitLab server of the New York state government was exposed to the internet, allowing anyone to self-register and log in to the system, which stored sensitive secrets.

    [https://techcrunch.com/2021/06/24/an-internal-code-repo-used-by-new-york-states-it-office-was-exposed-online/](https://techcrunch.com/2021/06/24/an-internal-code-repo-used-by-new-york-states-it-office-was-exposed-online/)
