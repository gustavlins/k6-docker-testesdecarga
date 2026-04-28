#  Teste de Carga Distribuído com k6 e Kubernetes

Este projeto é um laboratório prático de Engenharia de Performance. O objetivo é demonstrar como orquestrar e executar testes de carga utilizando o **k6** de forma distribuída dentro de um cluster **Kubernetes** local, utilizando o `k6-operator`.

## Pré-requisitos
Para rodar este projeto na sua máquina, você precisará ter instalado:
1.  [Docker Desktop](https://www.docker.com/products/docker-desktop/) (com a opção Kubernetes ativada).
2.  [kubectl](https://kubernetes.io/docs/tasks/tools/) configurado nas variáveis de ambiente.

## Como executar o projeto (Passo a Passo)

**1. Instalar o k6-operator no cluster:**
Isso adiciona a inteligência necessária para o Kubernetes entender testes do k6.
`kubectl apply -f https://raw.githubusercontent.com/grafana/k6-operator/main/bundle.yaml`

**2. Enviar o script de teste para o cluster (ConfigMap):**
O Kubernetes não lê arquivos locais, então empacotamos o script JS em um ConfigMap.
`kubectl create configmap rampvus --from-file=rampVUs.js`

**3. Disparar o Teste (TestRun):**
Aplica o manifesto YAML que define o paralelismo e inicia os Virtual Users (VUs).
`kubectl apply -f k6-testrun-resource.yaml`

**4. Acompanhar a execução:**
Para ver os "trabalhadores" (Pods) do k6 subindo e executando o teste em tempo real:
`kubectl get pods -w`

## Estrutura do YAML (`k6-testrun-resource.yaml`)
No arquivo de configuração, foi definido um `parallelism: 4`. Isso significa que o Kubernetes provisionou 4 pods distintos que dividiram a carga de teste simultaneamente, simulando um ambiente real de alta escalabilidade.
