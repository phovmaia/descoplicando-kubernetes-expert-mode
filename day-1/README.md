# instalando o kind

pre-requisitos para seguirmos com o nosso primeiro cluster é apenas ter o docker

para instalarmos o kind (Kubernetes in Docker) executamos o comando abaixo:

    curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.17.0/kind-linux-amd64
    chmod +x ./kind
    sudo mv ./kind /usr/local/bin/kind

o kind "simula" um cluster utilizando de containers como seus nodes, vamos criar nosso primeiro cluster com o comando abaixo:

    kind create cluster --name giropops

porém com o comando acima, o kind criara o cluster seguindo uma configuração default, agora vamos criar o cluster com a nossa configuração

vamos criar um arquivo [meu-primeiro-cluster.yaml](/day-1/kind/meu-primeiro-cluster.yaml) e executar o comando abaixo:

    kind create cluster --name giropops --config meu-primeiro-cluster.yaml

agora estamos criando o cluster seguindo nossa configuração definindo que  o cluster terá 4 nodes, 1 master e 3 workers

para vermos os nodes do nosso cluster, executamos o comando abaixo:

    kubectl get nodes

e terá um output parecido com

    NAME                     STATUS     ROLES           AGE   VERSION
    giropops-control-plane   Ready      control-plane   44s   v1.25.3
    giropops-worker          Ready      <none>          19s   v1.25.3
    giropops-worker2         NotReady   <none>          6s    v1.25.3
    giropops-worker3         Ready      <none>          19s   v1.25.3

# criando nosso primeiro pod

agora que temos nosso cluster com nossos nodes vamos criar nosso primeiro pod
o kubectl, tem um uma opção chamada dry-run que nos permite ver o que o kubectl irá enviar para o cluster, vamos usar essa opção para vermos o que o kubectl irá enviar para o cluster quando pedirmos para criar um pod com a imagem nginx e vamos salvar esse output em um arquivo chamado meu-primeiro-pod.yaml

    kubectl run nginx --image=nginx --dry-run=client -o yaml > meu-primeiro-pod.yaml

agora que criamos o arquivo podemos executar o comando

    kubectl apply -f meu-primeiro-pod.yaml

e teremos nosso primeiro pod criado, quando executarmos

    kubectl get pods

teremos um output parecido com

    NAME             READY   STATUS    RESTARTS   AGE
    giropops-nginx   1/1     Running   0          4m25s
