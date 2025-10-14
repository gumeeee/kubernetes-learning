# Projeto Kubernetes para o Curso "Iniciando com Kubernetes"

## Arquitetura

A aplicação é dividida em três componentes principais, cada um executado em seu próprio Pod no Kubernetes:

1.  **Banco de Dados (`db-noticias`)**: Um contêiner MySQL que armazena as notícias.
2.  **Sistema Backend (`sistema-noticias`)**: Uma aplicação backend que se conecta ao banco de dados para buscar as notícias.
3.  **Portal Frontend (`portal-noticias`)**: Uma aplicação frontend que exibe as notícias para o usuário, consumindo os dados do sistema backend.

## Componentes

Cada componente possui seus próprios arquivos de manifesto do Kubernetes:

### 1. Banco de Dados

*   **`db-configmap.yaml`**: Armazena as configurações sensíveis do banco de dados, como senhas e nome do banco.
*   **`db-noticias.yaml`**: Define o Pod que executa a imagem `aluracursos/mysql-db:1`.
*   **`svc-db-noticias.yaml`**: Cria um serviço do tipo `ClusterIP` para expor o banco de dados para os outros componentes dentro do cluster na porta `3306`.

### 2. Sistema Backend

*   **`sistema-configmap.yaml`**: Contém as configurações para o backend se conectar ao banco de dados.
*   **`sistema-noticias.yaml`**: Define o Pod que executa a imagem `aluracursos/sistem-noticias:1`.
*   **`svc-sistema-noticias.yaml`**: Cria um serviço do tipo `NodePort` para expor o backend no nó do Kubernetes na porta `30001`.

### 3. Portal Frontend

*   **`portal-configmap.yaml`**: Armazena a URL do serviço de backend.
*   **`portal-noticias.yaml`**: Define o Pod que executa a imagem `aluracursos/portal-noticias:1`.
*   **`svc-portal-noticias.yaml`**: Cria um serviço do tipo `NodePort` para expor o frontend no nó do Kubernetes na porta `30000`.

## Como Implantar

Para implantar a aplicação completa, você pode aplicar todos os arquivos de manifesto de uma vez com o seguinte comando:

```bash
kubectl apply -f .
```

Ou, se preferir, pode aplicar os arquivos em uma ordem específica para garantir que as dependências (como os ConfigMaps e Serviços) sejam criadas antes dos Pods que as utilizam:

1.  **ConfigMaps:**
    ```bash
    kubectl apply -f db-configmap.yaml
    kubectl apply -f sistema-configmap.yaml
    kubectl apply -f portal-configmap.yaml
    ```

2.  **Serviços:**
    ```bash
    kubectl apply -f svc-db-noticias.yaml
    kubectl apply -f svc-sistema-noticias.yaml
    kubectl apply -f svc-portal-noticias.yaml
    ```

3.  **Pods:**
    ```bash
    kubectl apply -f db-noticias.yaml
    kubectl apply -f sistema-noticias.yaml
    kubectl apply -f portal-noticias.yaml
    ```

## Acessando a Aplicação

Após a implantação, você pode acessar o portal de notícias através do endereço IP de qualquer nó do seu cluster Kubernetes, na porta `30000`.

**URL de acesso:** `http://<IP_DO_NO_DO_CLUSTER>:30000`
