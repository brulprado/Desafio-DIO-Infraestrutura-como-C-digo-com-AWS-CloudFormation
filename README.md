# Desafio-DIO-Infraestrutura-como-C-digo-com-AWS-CloudFormation
☁️ Projeto do Bootcamp CodeGirls AWS (DIO) implementando uma stack CloudFormation (IaC) para provisionar uma instância EC2 com servidor web Apache.

# ☁️ Implementando Minha Primeira Stack com AWS CloudFormation

## Bootcamp CodeGirls Santander e AWS - Desafio de Projeto DIO

**Status:** Concluído com Sucesso

### 🌟 Introdução

**Autora:** Bruna Lima Prado

Este repositório documenta a implementação bem-sucedida da minha primeira Stack na AWS utilizando o serviço **CloudFormation**. O objetivo principal foi provisionar uma instância EC2 (Elastic Compute Cloud) com um servidor web Apache instalado e em execução, utilizando um template **YAML** para descrever a infraestrutura como código (IaC).

---

### 💡 Objetivos de Aprendizagem e Conceitos Aplicados

* **Infraestrutura como Código (IaC):** Utilização de um template CloudFormation para definir, provisionar e gerenciar recursos de infraestrutura de forma declarativa e repetível.
* **AWS CloudFormation:** Entendimento e aplicação do ciclo de vida de uma Stack (criação, eventos, status) e uso do formato YAML para estruturar o template.
* **Provisionamento de Instância EC2:** Definição de recursos AWS (`AWS::EC2::Instance`) especificando propriedades como `ImageId`, `InstanceType`, `Tags` e `AvailabilityZone`.
* **User Data para Bootstrapping:** Uso da propriedade `UserData` para executar um script de inicialização na instância EC2, automatizando a instalação e configuração do servidor web Apache.
* **Funções Intrínsicas do CloudFormation:** Aplicação da função intrínseca `Fn::Base64` para codificar o script de `UserData`.

---

### ⚙️ Template CloudFormation (firewall.yaml)

O template foi desenvolvido em YAML para criar um recurso principal: uma instância EC2.

#### **Estrutura e Detalhes do Template:**

| Seção | Descrição |
| :--- | :--- |
| **`Description`** | Define a finalidade da Stack: "Instalar Servidor Apache". |
| **`Resources`** | Contém a definição do recurso `MinhaInstancia` do tipo `AWS::EC2::Instance`. |
| **`Properties`** | Define as configurações da EC2: `AvailabilityZone`, `ImageId` (AMI), `InstanceType` (`t3.micro`), e `Tags`. |
| **`UserData`** | Contém o script de *bootstrapping* para a instalação do Apache. |

#### **Script de Instalação (UserData):**

O script garante que o servidor Apache seja instalado, iniciado e configurado para iniciar automaticamente no boot da instância.

```bash
#!/bin/bash -xe
yum install -y httpd.x86_64
systemctl start httpd.service
systemctl enable httpd.service
# Cria um arquivo index.html simples
echo "<h1>DIO X AWS FOUNDATIONS do $(hostname -f) </h1>" > /var/www/html/index.html

Nota: A função intrínseca Fn::Base64: !Sub | foi utilizada para codificar este script, conforme exigido pela propriedade UserData do CloudFormation.

🖼️ Documentação da Execução
As capturas de tela a seguir comprovam a criação da Stack e o provisionamento dos recursos na AWS.

1. Template CloudFormation em YAML
Visualização do template que descreve o recurso EC2 e o script de instalação do Apache.

2. Configuração da Stack
Etapa de configuração das opções da Stack, incluindo a atribuição de Tags (laboratorio: ec2).

3. Eventos da Stack (Criação e Conclusão)
Registro dos eventos que mostram o início da criação da Stack (CRIAR_EM_ANDAMENTO) e a conclusão do provisionamento dos recursos (CRIAR_COMPLETO).

4. Validação do Recurso EC2
Confirmação no console EC2 de que a instância foi provisionada, está no estado "Executando" e possui o ID de instância gerado pelo CloudFormation.

Tipo de Instância: t3.micro

Zona de Disponibilidade: us-east-1a

✅ Conclusão e Insights
A execução deste desafio demonstrou a eficiência e a repetibilidade de se utilizar a Infraestrutura como Código. O CloudFormation abstrai a complexidade do provisionamento manual, permitindo que toda a infraestrutura (EC2, rede, security groups, etc.) seja gerenciada como código.

A utilização de UserData foi crucial para transformar uma instância "nua" em um servidor web funcional em um único deploy.

Acompanhar os Eventos da Stack é a forma mais eficaz de depurar e confirmar o sucesso do provisionamento.

A principal vantagem do CloudFormation é a garantia de que o ambiente pode ser replicado de forma idêntica e rápida, um pilar fundamental da filosofia DevOps.

📝 Próximos Passos (Avançando o Projeto)
Adicionar um recurso AWS::EC2::SecurityGroup ao template para configurar regras de acesso à porta 80 (HTTP).

Utilizar Outputs no template para exportar o DNS Público da instância EC2.

Introduzir Parameters para permitir a personalização do InstanceType ou ImageId durante a criação da Stack.
