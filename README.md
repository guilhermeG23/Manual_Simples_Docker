# Docker
## Manual simples

-------------------------------------------------------------------

### Agradecimentos

Esse manual simples foi gerado como um compilado de sites vistados, vídeos, experiencia pessoal e demais, com o intuito de ser um ponto de partido e uma referência sobre o tema a aqueles que desejarem, não tomando o escrito como minha própria autoria e sim algo como uma junção para a comunidade.

-------------------------------------------------------------------

### História
Antigamente o padrão era que cada servidor possui-se o fisico, o sistema e a aplicação, para cada nova aplicação ou sistema, um novo servidor era criado, ação que gerava: 

* Uma grande quantidade de máquinas;
* Custo elevado, dificuldade na manutenção;
* Demora para compra de novos equipamento;
* Além do disperdicio de hardware de algumas máquinas;

Claro que tal cenário é atualmente impossivel, más lembre-mos que já existiram memórias de 512MB, processadores single-core e HD IDE com 80MB de disco.

Voltando a normalidade, com o avançado do hardware, mais aplicações puraram ser executadas em um unico hardware, surgiu a virtualização para otimizar o uso de recursos por hardware e agora tem o Docker, a qual não é unico ou novo más extremamente competente com sua tarefa.

-------------------------------------------------------------------

### Diferença de uma V.M para um Container

Uma V.M (Virtual machine ou máquina virtual) simula um ambiente completo, isso é, um novo sistema operacional em cima do já atual, já um container é parte de um S.O, então faz alguma diferença?

**Uma V.M é um S.O inteiro novo, coisa que um container não é!**

Uma V.M virtualiza um S.O interno dentro do sistema atual, isso não é desejado por causa que é basicamente gasto de recursos atoa, digamos que precisamos somente de um interpretador BASH dentro da VM e foi instalado o Ubuntu inteiro com o GNOME, sabe aquele container que funcionaria com 256MB de RAM, essa V.M precisaria de uns 2GB de RAM.

Tem outras demais diferenças extremamente importante, más de forma resumida, imagine que ambos atendem mercados diferentes.

**Vantagens do container**
* Por não ser um OS completo o tamanho ocupado é pequeno;
* Extremamente mais leve que uma VM;
* Pode ser criado facilmente e destruido da mesma forma;

-------------------------------------------------------------------

### Motivos da virtualização e container

Pode que não colocar as aplicações direto no sistema? Em desktop caseiros estes problemas são raros, mas um caso industrial existe problemas mais expecificos, exemplos abaixo:

* Aplicações tentando sair pela mesma por ao mesmo tempo
* Se uma aplicação começar a consumir todos os recursos, o server para e você não consegue controlar facilmente;
* Dependencias expecificas para cada aplicação, se um utiliza a versão 1.2 e o outro 1.3, isso pode causa conflito, porque não pode usar mais de uma versão de determinada aplicação no sistema, claro existe o java, mas é um caso bem expecifico que requer uma configuração cuidado para não dar merda no sistema após isso;
* Sistema congela, reboot, ta certo, reboot o server que mantem a produção, vai dar super-certo isso, ou melhor o server do financeiro, uau, é dai pro RH se isso acontecer;

-------------------------------------------------------------------

### Sobre o Docker

Existem dois tipos de Docker, a empresa que foi renomeada para o nome do software após o boom, antes disso, a mesma se chamava dotcloud e existe o software, o boom em si.

O que acontecia na dotcloud é que eles usavam o amazon para contratar uma máquinas e usavam o dockerpara instanciar recursos, assim o Docker é um cara que gerencia e cria os conteiners sobre demanda.

-------------------------------------------------------------------

### Tecnologia
* Ferramentas moddenas para deployar e rodar aplicações;
* Docker engine -> é o cara que faz o intermedio do sistema e dos containers;
* Docker compose -> Facilitar a manipulações de multiplos containers de uma vez(orquestra);
* Docker swarm -> multiplos docker para criar um cluster;
* Docker hub -> repositorio com mais de 250 mil imagens de conteiners;
* Docker machine -> instalar e gerenciar hosts virtuais;

-------------------------------------------------------------------

### Usando o Docker
#### O Hello world, ALELUIA IRMÂOS E IRMÃS

Mostrando um hello world:

```
docker run hello-world
```

**OBS:** Quando você executar o docker vai procurar o container se ele existe na máquina, caso não achar ele baixa o container automaticamente do docker hub.

A saida:

```
root@teste-teste:/home/guilherme# docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete 
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
Status: Downloaded newer image for hello-world:latest
```

**Agora explicando:**

* **Comando** -> root@teste-teste:/home/guilherme# docker run hello-world
* **Procura interna** -> Unable to find image 'hello-world:latest' locally
* **Achando no docker hub** -> latest: Pulling from library/hello-world
* **Download no hello-word** -> 
    * 1b930d010525: Pull complete 
    * Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
* **Status** -> Status: Downloaded newer image for hello-world:latest

A sáida da crianção do container docker hello-world:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

-------------------------------------------------------------------

### Comandos básicos com containers

Mostra todas os containers ativos
```
docker ps
```

Mostra todos os containers já criados
```
docker ps -a
````

Exemplo do PS:

```
root@teste-teste:/home/guilherme# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                         PORTS               NAMES
bee82acef11a        hello-world         "/hello"            4 minutes ago       Exited (0) 4 minutes ago                           frosty_herschel
fd953eab9b1f        hello-world         "/hello"            About an hour ago   Exited (0) About an hour ago                       heuristic_pascal
```

#### Explicando o **PS -A**
* **CONTAINER ID** -> Numero do container, nome dele, sua identificação;
* **IMAGE** -> Container é baseado a qual imagem;
* **COMMAND** -> Comando que inicia o container;
* **CREATED** -> A que horas foi criado o container;
* **STATUS** -> O estado do container;
* **PORTS** -> Portas que o container está utilizando para saída;
* **NAMES** -> Nome RANDOM que o Docker dá para o container criado;

-------------------------------------------------------------------

### Como criar um container

Para se criar um container, pode-se usar o comando abaixo, necessita ter a idéia de qual imagem vai ser usada e o comando para iniciar o mesmo, o comando é opicional, maioria dos containers sobre com os /bin/bash ou executando o software desejado.

```
docker run <imagem> <comando>
```

Outro exemplo:

```
docker run ubuntu echo "hello world"
```

-------------------------------------------------------------------

### Sumiu?

Após executar o  container, pode ser que ele "feche", ele não fecha, ela fica em standbye esperando para ser chamado denovo, um jeito de voltar a usar o container criado é com o comando abaixo:

```
docker run -it <container>
```

Com isso você entra no container, e pode utiliza-lo como um linux normal por exemplo.

-------------------------------------------------------------------

### Agora quero sair do container

Aperte:

```
ctrl + d
```

Uma questão, você saiu do container, más ele ainda está rodando, para fechar um container por completo, necessita dar um exit no shell do mesmo ou força-lo a uma saida, assim, o container é fechado.

-------------------------------------------------------------------

#Tenho um container parado, o que fazer?

Pode usa-lo denovo, de um "ps -a" para ver os container's e pegue o ID do que queira subir

```
docker start <ID>
```

Ex:
```
docker start bee82acef11a
```

Quero iniciar um contianer atrelando o terminal

```
docker start -a <ID>
```

-------------------------------------------------------------------


### Quero parar um container sem entrar nele

Execute:
```
docker stop <ID>
```

-------------------------------------------------------------------

### Comandos para dentro de um container

Quero executar um comando no container

```
docker exec -ti <id> echo "teste"
```

-------------------------------------------------------------------

###Remover um container

Com base em um ID é possível deletar um container já utilizado

```
docker rm <id>
```

Remover um container a força, mesmo que o container esteja rodando ou sendo utilizado por outros como base.

```
docker rm -f <id>
```

No caso do rm não precisa colocar todo o id, somente os primeiros caracteres que já o diferencie dos outros já basta

**Exe:**

Listado os containers:

```
root@teste:/home/teste# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
903e615ddc18        ubuntu              "/bin/bash"         7 minutes ago       Exited (0) 7 minutes ago                        romantic_golick
de63c97fd1b1        ubuntu              "echo teste"        8 minutes ago       Exited (0) 8 minutes ago                        quirky_tereshkova
e55c0f28dc7e        ubuntu              "/bin/bash"         11 minutes ago      Exited (0) 8 minutes ago                        wizardly_wright
bee82acef11a        hello-world         "/hello"            30 minutes ago      Exited (0) 30 minutes ago                       frosty_herschel

```

Removendo via ID:

```
root@teste:/home/teste# docker rm e55c0f28dc7e
e55c0f28dc7e
```

Removendo a partir dos valores diferentes:

```
root@guilherme-340XAA-350XAA-550XAA:/home/guilherme# docker rm de63
de63
```

O Resultado:

```
root@guilherme-340XAA-350XAA-550XAA:/home/guilherme# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
903e615ddc18        ubuntu              "/bin/bash"         8 minutes ago       Exited (0) 8 minutes ago                        romantic_golick
bee82acef11a        hello-world         "/hello"            31 minutes ago      Exited (0) 30 minutes ago                       frosty_herschel
```

Agora deletando todos os containers parados:

```
docker container prune
```

-------------------------------------------------------------------

### Imagens dos containers

Mostrar as imagens já salvas no sistema

```
docker images
```

Remover uma imagem

```
docker rmi <nome da image>
```

-------------------------------------------------------------------

### O conceito de camadas


O docker possui um sistema de pull em camadas, quanto mais completa foi a imagem mais pulls ela vai ter, um exemplo disso é a imagem **hello-world** que só possui 1 pull, enquanto o ubuntu possui 4 pulls minimos.


**Exemplo:** 
* Vamos falar que você baixou uma imagem e ela possue 5 camadas e foram feitos 5 pulls;
* Agora baixa outra imagem parecida com a anterior e ela só mostra 2 pulls;

#### porque tem diferença?

O docker possui o sistema de camadas de download, onde uma imagem é feita por pulls, quando baixamos uma imagem ele verifica que pulls vai fazer e caso voce já possui essa camada, ele ignora e utiliza a do sistema já existente;

As camadas são só para leitura e não para a escrita, o docker possue um conceito de sobrepor essas camadas, quando uma alteração é feita uma camada nova é colocada em cima das anteriores e é possivel alterar o estado.

O nome disso é:
* **Read/write layer** -> Essa é a camada que o usuario do sistema tem permissão para mexer;

O conceito de container tambem funciona dessa forma, voce inicia um container mas a imagem é sempre a mesma, assim sempre que um container novo é colocado no ar, ele utiliza a base e só cria a camada para o usuario editar.

-------------------------------------------------------------------

### Docker mais prático

#### Subindo um server web

```
docker run dockersamples/static-site
```

Você vai notar que ele não volta para o terminal, porque esse container é de execução continua, esta rodando um web server, então ele não vai parar a mesma que você o mate.

Executar um conteinar sem entrar nele quando executado:

```
docker run -d <imagem>
```

O  **-d** executa o container em modo detached, o container ainda continua funcionado, porém você se mantem no seu terminal;

-------------------------------------------------------------------

### Parar o container

Pode se executar um container e para-lo após alguém tempo, por padrão o container para após 10 segundos.

```
docker stop -t <tempo em segundos> ID
```

-------------------------------------------------------------------

### Abrir uma porta

Liberar uma porta do computador para fazer a conexão com o container

```
docker run -d -P <porta> <imagem>
```

O **-P** atribiu uma porta externa qualquer outra do computador host;

Mostrar as portas de um container

```
docker port <id da imagem rodando>
```

Saída:

```
root@teste:/home/teste# docker port 1eee35006ca9
443/tcp -> 0.0.0.0:32768
80/tcp -> 0.0.0.0:32769
```

Mostrar o ip da maquina-docker:

```
docker-machine ip
```

Atribuir um nome ao container:

```
docker run -d nome-container <imagem>
```

Você pode para um container a partir do nome dele, dessa forma é mais facil para um container com nome atribuido do que um com nome aleatorio.

-------------------------------------------------------------------

### Subir um teste

**Ex:**

```
docker run -d -p <porta da maquina>:<porta do container> <imagem>
```

**Saída:**

```
root@teste:/home/teste# docker run -d -p 8080:80 dockersamples/static-site
7a4a15eaf07f267e64b76ba72537a62fcb12e7bfd4fd45a7192ca24c3b494a3a
```

Atrelar o autor do container
```
docker run -d -P -e AUTHOR="guilherme" <imagem>
```

Listar os IDs dos containers
```
docker ps -q
```

-------------------------------------------------------------------

### Usando variaveis no docker 

Jeitinho brasileiro para controle de vários containers

Parar todos os containers:
```
docker stop -t 0 $(docker ps -q)
```

-------------------------------------------------------------------

### Lista de comandos:
* **docker ps** -> exibe todos os containers em execução no momento;
* **docker ps -a** -> exibe todos os containers, independente de estarem em execução ou não;
* **docker run -it NOME_DA_IMAGEM** -> conecta o terminal que estamos utilizando com o do container;
* **docker start ID_CONTAINER** -> inicia o container com id em questão;
* **docker stop ID_CONTAINER** -> interrompe o container com id em questão;
* **docker start -a -i ID_CONTAINER** -> inicia o container com id em questão e integra os terminais, além de permitir interação entre ambos;
* **docker rm ID_CONTAINER** -> remove o container com id em questão;
* **docker container prune** -> remove todos os containers que estão parados;
* **docker rmi NOME_DA_IMAGEM** -> remove a imagem passada como parâmetro;
* **docker run -d -P --name NOME dockersamples/static-site** -> ao executar, dá um nome ao container;
* **docker run -d -p 12345:80 dockersamples/static-site** -> define uma porta específica para ser atribuída à porta 80 do container, neste caso 12345;
* **docker run -d -e AUTHOR="Fulano" dockersamples/static-site** -> define uma variável de ambiente AUTHOR com o valor Fulano no container criado;

-------------------------------------------------------------------

### Inspecionar um container

Analisar o que aconteceu com o container:

```
docker inspect <id>
```

-------------------------------------------------------------------

### Volumes de dados dos containers

Volume é um local compartilhada do computador local ou externo que é aberto dentro do container, pasta externa que o docker suba no container, mesmo se apagar o container o arquivo ainda existe, os dados ficam salvos no docker host.

Monstando um volume:

```
docker run -v -d "/var/www" <imagem>
```

Compartilhando um diretorio

```
docker run -v "<caminho do pc host ou externo>:<caminho dentro do container>" <imagem>
```

**Exemplo:**
```
root@teste:/home/teste# docker run -it -v "/home/teste/:/home/" ubuntu
```

**LS de dentro do container:**
```
root@aebf270b462c:/home# ls
 Desenvolvimentos_Melhorias  'M'$'\303\272''sica'    'V'$'\303\255''deos'
 Documentos                   NetBeansProjects        examples.desktop
 Downloads                    PacketTracer_Projetos   pt
 GAME_RPG                     PycharmProjects         snap
 Imagens                     'P'$'\303\272''blico'   ''$'\303\201''rea de Trabalho'
 Modelos                     'VirtualBox VMs'
```

-------------------------------------------------------------------

### Exemplo para subir um server node 

docker run -d -p 8080:3000 -v "/home/guilherme/teste:/var/www" -w "/var/www" node npm start

-w = é um iniciar de determinado ponto

-------------------------------------------------------------------

### Dockefile

É um arquivo receita para subir container personalizado

```
FROM node:latest
MAINTAINER guilhermeG23
ENV PORT=3000
COPY exemplo/ /var/www
WORKDIR /var/www
RUN npm install
ENTRYPOINT npm start
EXPOSE $PORT
```

Explicando:

* **FROM** -> escolhe o software que a receita se base e 'lastest' é a versão mais recente;
```
FROM node:latest
```

* **MAINTAINER** -> Autor e quem mantem o arquivo;

```
MAINTAINER guilhermeG23
```

* **ENV** ->Criando uma variavel;
```
ENV PORT=3000
```

* **COPY** -> Copiar e colar arquivo dentro do projeto;
```
COPY exemplo/ /var/www
```

* **WORKDIR** -> Definir a pasta que a receita inicia;
```
WORKDIR /var/www
```

* **RUN**-> Executar comando;
```
RUN npm install
```

* **ENTRYPOINT** -> Executando ação apos iniciar;
```
ENTRYPOINT npm start
```

* **EXPOSE**->Expor a porta do container;
```
EXPOSE $PORT
```

* Pode se usar # como comentário dentro de um dockerfile


Para criar uma receita
```
docker build -f Dockefile -t <nome de saida> .
```

Quando build o projeto ele cria uma container intermediario para a determinada ação, cada comamdo no dockerfile gera um container intermediario que é deixado no container final. A criação de container no dockerfile segue o mesmo do docker run, toda a vez que for alterar uma imagem para sua criação, ele somente modificara a parte editada na receita, dessa forma não precisa editar outra camada

* **Exemplo**:
```
docker build -f Dockerfile -t saida/guilherme .
```

-------------------------------------------------------------------

###Colocar uma imagem no docker hub

* Entra no site, coloque ou crie sua conta
* Após isso va para o terminal e execute docker login, entre com usuario e senha, apos isso voce cai estar logado

Para subir a imagem no hub:
```
docker push <nome da imagem>
````

**Exemplo:**
```
docker push <nome do usuario do docker hub>/<nome da imagem>
```

Sobre as camadas dos containers o mesmo vale no push. Ele so sobe as camadas que for precisar e não sobe o que ele notar quee já existe, assim ele sobe so as camadas exenciais e as configuracao;


Para fazer o up da uma imagem 
```
docker pull <nome do usuario>/<imagem>
```

Ex:
```
docker pull guilhermebrechot/teste_ubuntu
```

-------------------------------------------------------------------

### Redes de containers

Conectando multiplos containers

**Exemplo***:
* Server web;
* aplicacao;
* Banco;
* Server de cache;

Todos interligados para criar o funcionamento, isso é cada container tem uma responsabilidade;

O docker já tem em mente o uso de redes, ele uma default network para o funcionamento dos containers e todos o containers são iniciados com ela, do 172.168.0.1 ao 254 uma rede / 24 interna do docker

**Exemplo da rede de um container:**
```
"NetworkSettings": {
            "Bridge": "",
            "SandboxID": "2a1285ce33d47a100353d07b0f4c4121f5ed969b09f237aec51ca277b3d8be2c",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "443/tcp": null,
                "80/tcp": null
            },
            "SandboxKey": "/var/run/docker/netns/2a1285ce33d4",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "dedef995f2c691296c2901c6292be1c03b7a1e9180ad39203439c05aee197e0d",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "cddc52925969a071445ffd4739bf7ee7a2d88b6d1b0a16b59efca3e7067352c7",
                    "EndpointID": "dedef995f2c691296c2901c6292be1c03b7a1e9180ad39203439c05aee197e0d",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
```

Lembrando por que alguns comandos não funcionam dentro do container? O container so tem o essencial para funcionar, comandos desnecessarios nem sao colocados no container, no caso você precisa baixar os containers e lembrando que o container é temporario, caso entre em um container e faça alterações como instalar um pacote, isso será perdido no momento que o container ser morto

#### Provando a rede

Suba dois containers com ubuntu e instale os pacotes necessarios para o ping e ifconfig, o comando: "**apt update && apt install iputils-ping -y && apt install net-tools**", após subir os dois e instalar os comandos acima em um e pingue o outro

**Exemplo:**
```
root@c84a471a89dc:/# ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3) 56(84) bytes of data.
64 bytes from 172.17.0.3: icmp_seq=1 ttl=64 time=0.224 ms
64 bytes from 172.17.0.3: icmp_seq=2 ttl=64 time=0.131 ms
64 bytes from 172.17.0.3: icmp_seq=3 ttl=64 time=0.111 ms
64 bytes from 172.17.0.3: icmp_seq=4 ttl=64 time=0.116 ms
64 bytes from 172.17.0.3: icmp_seq=5 ttl=64 time=0.113 ms
64 bytes from 172.17.0.3: icmp_seq=6 ttl=64 time=0.114 ms
```

Para configurar a rede, isso é feito no docker hosts, o containers não precisa se conectar na rede default do sistema, eles se comunicam via docker hosts como uma camada de abstração

#### Criando rede no docker

O drive é como criar uma nuvem particular entre os containers, o bridge é o mais comum e resolve maiorias dos problemas, rede-containers é o nome da rede

**Exemplo:**
```
docker network create --driver bridge rede-containers
```

Mostrar as maquinas em rede do docker:
```
docker network ls
```

Associar uma rede ao container:
```
docker run ubuntu --name nome-container --network redes-containers
```

* **--network** -> fala para o container ingressar em determinada rede;

Quando criamos a rede e colocamos um nome no container, podemos pingar ele pelo nome por que define este nome como um host da rede

#### Criada a rede 

Criada duas maquina e colocadas hostnames diferentes(obvio) e colocados na mesma rede, uma delas teve os pacotes de rede instalados e foi testado os comandos.

Teste do host teste para o teste1:

```
root@811961491abf:/# ping teste1
PING teste1 (172.18.0.3) 56(84) bytes of data.
64 bytes from teste1.rede-containers (172.18.0.3): icmp_seq=1 ttl=64 time=0.222 ms
64 bytes from teste1.rede-containers (172.18.0.3): icmp_seq=2 ttl=64 time=0.127 ms
64 bytes from teste1.rede-containers (172.18.0.3): icmp_seq=3 ttl=64 time=0.125 ms
```

Resumindo, você não mexe na rede interna do docker e sim no docker host onde você cria uma rede e atribui onde as máquina seram iniciadas com determinada rede, por padrão os containers sobem com uma default mas da para setar a rede que você quer subir usando --network, lembrando que tem que atribuir um nome para os containers para facilitar o gerenciamento


#### Aplicação

Executando uma aplicação que utiliza 2 containers:
```
docker pull douglasq/alura-books:cap05
docker pull mongo
```

Subir os dockers:
```
docker network create --driver bridge minha-rede
docker run -d --network minha-rede --name meu-mongo mongo
docker run -d -p 8080:3000 --network minha-rede --name meu-node douglasq/alura-books:cap05
```

Quando você abrir o localhost:8080 ele vai mostrar nada, tem que entrar numa pagina para fazer os livros subirem na aplicação
```
localhost:8080/seed
```

Após isso volte para o localhost:8080 que o livros seram carregados

Porém isso prova que ambos os containers estão conversando entre si.

Analise da rede, Mostra o que tem dentro duma rede além de outros atributo
```
docker network inspect <nome da rede>
```

-------------------------------------------------------------------

### Docker compose

Até agora subimos todos os containers na mão, agora vai automatizar este processo para que não seja repetitivo, cansativo, cheio de falhas e lento, não que subir na mão seja ruim, mas não recomendavel utilizar em chão de fabrica por exemplo, quanto mais automatizado melhor o gerenciamento, containers não foram feitos só para subir um e alguentar o trafego e sim, subir varios e balancear o trafego entre eles

Exemplificando uma rede com container de modo mais correto

Container para gerenciar as conexoes

* **local balancer** -> nginx(fazer as regras) -> normalmente deixa o static aqui como html, e imagens
* **Aplicacao** -> inumeros containers para rodar a aplicacao -> roda somente a função da pagina(lógica do negócio)
* **banco** -> inumeros banco para manter a aplicação

#### Compose

Uso do compose e arquivo .yml, utiliza arquivos: docker-compose.yml, o compose usa um arquivo .yml como uma receita de multiplos containers

* **.yml** -> parecido com um .json mas com a identação do python

Executando o compose
```
docker-compose build <caminho do arquivo.yml>
```

Executa as instruções do .yml

Iniciando o Servico
```
docker-compose up
```

Exemplo de arquivo docker-compose.yml

Versao do docker-compose executada
```
version: '3'
```

Criando uma rede
```
networks:
  production-network:
    driver: bridge
```

Servicos são os containers

```
# - podem receber mais de 1 item na listagem
services:

  #Banco de dados
  mongodb:
    image: mongo
    network:
      - production-network

  #Web server
  nginx:
    build:
      #Procura o docker file que é para ser executado
      dockerfile: ./dockerqnginx.dockerfile
      #Inicia a produca de que ponto
      #Inicia do diretorio atual
      context: .
    #Nome da imagem
    image: guilhermebrechot/nginx
    #Nome do container
    container_name: nginx
    #Expose das postas
    ports:
      #8080 porta do desktop
      #80 porta do container
      - "8080:80"
    #Expecificando a rede
    networks: 
      - production-network
    #depende de ter subido:
    depends_on:
      - "node1"
      - "node2"
      - "node3"
```

depends é necessario, se uma coisa precisa funciona mas ela depende de outra, ela so sobe depois que o que depente já esta funcionando.

```
node1:
  build:
    dockerfile: ./docker/alura-books.dockerfile
    context: .
  image: guilhermebrechot/nodes
  container_name: node1
  ports:
    - "3000"
  network:
    - production-network
  depends_on:
    - "mongodb"
node2:
  build:
    dockerfile: ./docker/alura-books.dockerfile
    context: .
  image: guilhermebrechot/nodes
  container_name: node2
  ports:
    - "3000"
  network:
    - production-network
  depends_on:
    - "mongodb"

node3:
  build:
    dockerfile: ./docker/alura-books.dockerfile
    context: .
  image: guilhermebrechot/nodes
  container_name: node3
  ports:
    - "3000"
  network:
    - production-network
  depends_on:
    - "mongodb"
```

Se você fechar a aplicação enquanto voce tiver nela, todos os containers do compose são parados

-d no compose
```
docker-compose up -da
```

Listar o compose
```
docker-compose ps
```

Parar e remover todos os containers do compose
```
docker-compose down
```

Reiniciar container parados
```
docker-compose restart
````

-------------------------------------------------------------------

### Ajuda

Com certeza essa é o comando mais amigo que você vai achar

```
docker --help
docker-compose --help
```
