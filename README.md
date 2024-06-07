# Criando-um-Sistema-de-Or-amento-Utilizando-CQRS-Quarkus-Kafka-e-Deploy-no-EKS

Para criar um sistema de orçamento utilizando CQRS, Quarkus, Kafka e deploy no EKS, você pode seguir os seguintes módulos:

### DESCRIÇÃO DO PROJETO:

### Módulo 1: Configuração Inicial
- **Configuração do Ambiente**: Instale as ferramentas necessárias como Java/Kotlin, Docker, AWS CLI, kubectl e eksctl.
- **Criação do Cluster EKS**: Utilize o `eksctl` para criar um cluster no EKS.

### Módulo 2: Desenvolvimento dos Serviços Quarkus
- **Criação dos Projetos Quarkus**: Utilize o Maven ou Gradle para criar dois projetos Quarkus separados, um para o comando e outro para a consulta.
- **Implementação do CQRS**: Defina os comandos e consultas dentro dos serviços Quarkus.

### Módulo 3: Integração com Kafka
- **Configuração do Kafka**: Configure o Kafka para ser o barramento de mensagens entre os serviços.
- **Implementação da Comunicação Assíncrona**: Utilize o Kafka para enviar e receber mensagens entre os serviços.

### Módulo 4: Criação dos Manifestos do Kubernetes
- **Deployment dos Serviços**: Escreva os manifestos de deployment para os serviços Quarkus.
- **Configuração dos Serviços**: Defina os serviços do Kubernetes para expor os serviços Quarkus.

### Módulo 5: Deploy no EKS
- **Deploy dos Manifestos**: Utilize o `kubectl` para aplicar os manifestos no cluster EKS.
- **Verificação**: Verifique se os serviços estão rodando corretamente no EKS.

### Módulo 6: Monitoramento e Logging
- **Configuração do Monitoramento**: Configure ferramentas como Prometheus e Grafana para monitorar os serviços.
- **Configuração de Logging**: Configure o Fluentd ou outro agregador de logs para enviar logs para um serviço como o Elasticsearch.

Este projeto visa implantar uma aplicação Java/Kotlin no serviço Elastic Kubernetes Service (EKS) da Amazon. A aplicação exemplifica o padrão Command Query Responsibility Segregation (CQRS) com dois serviços Quarkus que se comunicam por meio de um barramento assíncrono usando Kafka. Você aprenderá a criar os manifestos do Kubernetes para implantação no EKS e as configurações necessárias para ter o ambiente funcionando em produção.

**Objetivos**:
- Implementar o padrão CQRS em uma aplicação Java/Kotlin.
- Utilizar Quarkus para criar serviços leves e eficientes.
- Configurar o Kafka como um barramento de mensagens assíncrono.
- Realizar o deploy da aplicação no EKS e garantir sua execução em produção.

**Resultado Esperado**:
Ao final do projeto, você terá um sistema de orçamento rodando no EKS, com uma arquitetura baseada em CQRS e comunicação assíncrona via Kafka.

Aqui estão alguns exemplos de código para os módulos:

### Módulo 2: Exemplo de Código Quarkus
```java
// ComandoService.java
@ApplicationScoped
public class ComandoService {

    @Inject
    Event<OrçamentoCriado> eventoOrçamentoCriado;

    public void criarOrçamento(Orçamento orçamento) {
        // Lógica para criar um orçamento
        eventoOrçamentoCriado.fire(new OrçamentoCriado(orçamento));
    }
}

// ConsultaService.java
@ApplicationScoped
public class ConsultaService {

    private List<Orçamento> orçamentos = new ArrayList<>();

    public List<Orçamento> listarOrçamentos() {
        return orçamentos;
    }

    public void onOrçamentoCriado(@Observes OrçamentoCriado event) {
        orçamentos.add(event.getOrçamento());
    }
}
```

### Módulo 4: Exemplo de Manifesto Kubernetes
```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: comando-service
spec:
  replicas: 1
  selector:
    matchLabels:
      app: comando-service
  template:
    metadata:
      labels:
        app: comando-service
    spec:
      containers:
      - name: comando-service
        image: comando-service:latest
        ports:
        - containerPort: 8080
```

Lembre-se de ajustar os códigos conforme necessário para o seu ambiente específico e a configuração da sua aplicação.
