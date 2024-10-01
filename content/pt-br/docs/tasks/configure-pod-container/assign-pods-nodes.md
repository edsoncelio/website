---
title: Atribuindo Pods a Nós
content_type: task
weight: 150
---

<!-- overview -->
Essa página mostra como atribuir um Pod a um nó em particular em um cluster Kubernetes.

## {{% heading "prerequisites" %}}


{{< include "task-tutorial-prereqs.md" >}} {{< version-check >}}



<!-- steps -->

## Adicionar uma label a um nó

1. Liste os {{< glossary_tooltip term_id="node" text="nós" >}} no seu cluster, justamente com as labels:

    ```shell
    kubectl get nodes --show-labels
    ```
    A saída é parecida com isso:

    ```shell
    NAME      STATUS    ROLES    AGE     VERSION        LABELS
    worker0   Ready     <none>   1d      v1.13.0        ...,kubernetes.io/hostname=worker0
    worker1   Ready     <none>   1d      v1.13.0        ...,kubernetes.io/hostname=worker1
    worker2   Ready     <none>   1d      v1.13.0        ...,kubernetes.io/hostname=worker2
    ```
1. Escolha um dos seus nós, e adicione uma label:    

    ```shell
    kubectl label nodes <your-node-name> disktype=ssd
    ```
    Onde `<your-node-name>` é o nome do nó que você escolheu.

1. Verifique se o nó escolhido está com a label `disktype=ssd`:

    ```shell
    kubectl get nodes --show-labels
    ```

    A saída é parecida com isso:

    ```shell
    NAME      STATUS    ROLES    AGE     VERSION        LABELS
    worker0   Ready     <none>   1d      v1.13.0        ...,disktype=ssd,kubernetes.io/hostname=worker0
    worker1   Ready     <none>   1d      v1.13.0        ...,kubernetes.io/hostname=worker1
    worker2   Ready     <none>   1d      v1.13.0        ...,kubernetes.io/hostname=worker2
    ```

    Na saída acima, é possível você ver que o nó `worker0` está com a label `disktype=ssd`.

## Criar o pod que vai ser agendado no nó escolhido

Esse arquivo de configuração descreve um pod que contém o seletor de nó `disktype: ssd`. 
Isso quer dizer que esse pod vai ser agendado em um nós que contenha a label `disktype: ssd`.

{{% code_sample file="pods/pod-nginx.yaml" %}}

1. Use o arquivo de configuração para criar um pod que vai ser agendado no nó escolhido:
    ```shell
    kubectl apply -f https://k8s.io/examples/pods/pod-nginx.yaml
    ```

1. Verifique se o pod está executando no nó escolhido:

    ```shell
    kubectl get pods --output=wide
    ```

    A saída é parecida com isso:
    
    ```shell
    NAME     READY     STATUS    RESTARTS   AGE    IP           NODE
    nginx    1/1       Running   0          13s    10.200.0.4   worker0
    ```
## Criar um pod que vai ser agendado em um nó específico

Você também pode agendar um pod em um nó específico configurando `nodeName`.

{{% code_sample file="pods/pod-nginx-specific-node.yaml" %}}

Use esse arquivo de configuração para criar um pod que vai ser agendado apenas no nó `foo-node`.


## {{% heading "whatsnext" %}}

* Saiba mais sobre [labels e seletores](/docs/concepts/overview/working-with-objects/labels/).
* Saiba mais sobre nós [nodes](/docs/concepts/architecture/nodes/).


