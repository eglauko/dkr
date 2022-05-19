# Docker

Docker Scripts and docker compose files.

## Instalando o VS Code, Docker, Compose e Minikube no WSL2

### Pré-requisitos

Os procedimentos abaixo requerem que já se tenha uma distribuição linux instalada para o WSL2.

Os comandos são baseados na distribuição do Ubuntu.

Atualize o sistema com os comandos:

```
sudo apt update && sudo apt upgrade
```


### Instalando o VS Code

Basicamente é só seguir as instruções deste [link da MicroSoft](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode)

Em resumo, deve ser instalado o VS Code no windows e adicionar a extensão [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)

Em seguida, abra o terminado do wsl e digite `code .` ou `code {diretório}`

Será baixado alguns arquivos necessário e depois o VS Code será aberto no windows com o diretório do WSL aberto.

Para dar permissão de acesso a criação e edição de arquivos e diretórios no WSL a partir do windows (VS Code), utilize o comando `chmod`, como por exemplo:

```
sudo chmod 777 ./dir -R
```


### Instalando o Docker

Primeiramente é necessário remover qualquer componente que já possa estar instalado:

```
sudo apt remove docker docker-engine docker.io containerd runc
```

Segundo, é necessário instalar as dependências:

```
sudo apt install --no-install-recommends apt-transport-https ca-certificates curl gnupg2
```

Em seguida devemos atualizar os repositórios:

```
. /etc/os-release

curl -fsSL https://download.docker.com/linux/${ID}/gpg | sudo tee /etc/apt/trusted.gpg.d/docker.asc

echo "deb [arch=amd64] https://download.docker.com/linux/${ID} ${VERSION_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/docker.list

sudo apt update
```

Então, podemos instalar o Docker:

```
sudo apt install docker-ce docker-ce-cli containerd.io
```

Agora que o Docker está instalado, é necessário iniciar o serviço.

Toda vez que o computador for reiniciado, será necessário iniciar o serviço do docker.

Utilize o comando para iniciar o docker:

```
sudo service docker start
```

Uma vez iniciado o serviço do docker, é possível usá-lo. Para testar use qualquer comando, como por exemplo o `ps`.

Normalmente ocorrerá um erro se não usar o `sudo`, por exemplo `docker ps` dará uma mensagem de erro de permissão:

```
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/json": dial unix /var/run/docker.sock: connect: permission denied
```

então é necessário dar permissão ao usuário docker:

```
sudo usermod -aG docker $USER
```

Feche o terminal e abra um novo, então poderá executar `docker ps`.

Na versão 22 do Ubuntu ocorrerá um erro como este:

```
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

O motivo deste erro é porque o Ubuntu 22.04 LTS utiliza o `iptables-nft` por padrão.
Você precisa mudar para o `iptables-legacy` para que o Docker funcione novamente.

Execute o comando para configurar iptables:
```
sudo update-alternatives --config iptables
```

Será listada algumas opções, então, digite o número da opção `iptables-legacy`, o qual normalmente será `1`.

Depois disso, inicie o serviço do docker e execute algum comando para testar.

### Instalando o Docker-Compose

Uma vez instalado o docker, para utilizar o compose é super simples, apenas execute os comandos abaixo:

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```


### Instalando o Kubernetes - Minikube


Comandos para instalar o Minikube:

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb

sudo apt install ./minikube_latest_amd64.deb -y

rm minikube_latest_amd64.deb
```

Uma vez instalado é hora de iniciar o Minikube:

```
minikube start
```

Na primeira execução será feito o download do kubernetes e demorará mais tempo.

Para abrir o dashboard do kubernetes use o comando:

```
minikube dashboard
```

Será exibido um endereço que pode ser aberto no navegador no windows, semelhante a este:

```
http://127.0.0.1:[PORT]/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/
```

A porta sempre irá variar, então, depois de usar o comando, copie o link e o abra no navegador.

Para deixar o dashboard rodando em segundo plano, aperte `Ctrl + Z` no terminal e depois digite `bg`.

Agora é hora de instalar o kubectl, para isto, use os comandos:

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

sudo apt-get install -y kubectl
```

Feito isso o kubectl já pode ser utilizado, teste usando um comando qualquer, como por exemplo `kubectl get po -A`

Para finalizar, pode ser instalado o Helm, com estes comandos:

Obs.: Fique atento a versão que quer instalar.

```
curl --output helm-linux-amd64.tar.gz https://get.helm.sh/helm-v3.8.2-linux-amd64.tar.gz

tar -zxvf helm-linux-amd64.tar.gz

sudo mv linux-amd64/helm /usr/local/bin/helm

rm helm-linux-amd64.tar.gz

rm -rf linux-amd64
```

Para testar se deu tudo certo execute um comando qualquer, como por exemplo `helm version`.
