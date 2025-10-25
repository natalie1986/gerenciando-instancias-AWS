# Projeto AWS: Gerenciamento de Instâncias EC2

Repositório criado como parte do desafio de projeto do bootcamp **Santander Code Girls 2025**, uma parceria com a [Dio.me](https://www.dio.me/).

O objetivo deste projeto é documentar e consolidar os conceitos fundamentais sobre a nuvem AWS, com foco especial no gerenciamento de instâncias EC2, otimização de custos e boas práticas de segurança e armazenamento.

---

# Primeiros Passos na AWS: Fundamentos de Computação e Armazenamento

A Amazon Web Services (AWS) é uma plataforma de computação em nuvem que fornece mais de 60 produtos e serviços, desde servidores virtuais (instâncias) até bancos de dados e ferramentas de desenvolvimento. A computação em nuvem em geral é uma tecnologia usada para armazenamento remoto de dados e gerenciamento de dispositivos. Ao migrar para a nuvem, as organizações têm a flexibilidade de usar os recursos apenas conforme a necessidade, transformando custos fixos em despesas variáveis.

## 1. Fundamentos Essenciais da Infraestrutura AWS

A infraestrutura da AWS é projetada para ser globalmente disponível, resiliente e escalável.

### Regiões e Zonas de Disponibilidade (AZs)

Uma **Região da AWS** é um conjunto de recursos da AWS em uma área geográfica. Cada Região é isolada e independente das outras para fornecer tolerância a falhas, estabilidade e resiliência.

Dentro de cada Região, existem as **Zonas de Disponibilidade (AZs)**. Uma Zona de Disponibilidade é um local distinto isolado de falhas em outras zonas de disponibilidade, fornecendo conectividade de rede de baixa latência e baixo custo dentro da mesma Região.

### Amazon Virtual Private Cloud (VPC)

A Amazon VPC permite que você defina uma rede virtual em sua própria área isolada logicamente dentro da Nuvem AWS.

- **Sub-redes:** Você pode lançar recursos (como instâncias) em sua VPC, definindo sub-redes e tabelas de rotas. Uma sub-rede é um intervalo de endereços IP alocado a uma única Zona de Disponibilidade.
- **Segurança:** A VPC se assemelha a uma rede tradicional em seu próprio *data center*, com a diferença de usar a infraestrutura escalável da AWS. Para proteger recursos, você pode usar várias camadas de segurança, incluindo grupos de segurança e listas de controle de acesso de rede.

## 2. Configurando sua Conta AWS com Segurança

A segurança é um pilar fundamental da AWS. O gerenciamento correto de sua conta começa com a configuração inicial de acesso.

### Usuário Raiz (*Root User*) e IAM

Quando você se inscreve em uma Conta da AWS, o **Usuário Raiz** é criado, tendo acesso a todos os serviços e recursos da conta.

- **Prática Recomendada de Segurança:** Atribua o acesso administrativo a um usuário separado e use o Usuário Raiz **somente para executar tarefas que exigem acesso de usuário-raiz**.
- **IAM (Identity and Access Management):** O IAM centraliza o controle de acesso para as ações que podem ser realizadas em suas máquinas, seguindo o princípio de **privilégio mínimo**, concedendo apenas as permissões necessárias para uma tarefa.

### Tags e Otimização de Custos

**Tags** são pares chave-valor que atuam como metadados, ajudando a organizar seus recursos.

- **Gerenciamento de Custos:** As tags são essenciais para criar relatórios de custos que podem ser filtrados e consolidados por dimensões como centro de custo, tipo de ambiente (desenvolvimento ou produção) ou projeto.
- **Segurança:** O uso de tags personalizadas em recursos é uma prática recomendada para ajudar a determinar e implementar os requisitos de segurança e criptografia adequados, de acordo com as políticas da organização.

### Otimização de Custos: Ferramentas e Nível Gratuito

A AWS oferece várias formas de otimizar os custos, incluindo o uso de **ferramentas baseadas em dados** e modelos de precificação flexíveis.

- **AWS Trusted Advisor:** Uma ferramenta online que fornece orientações para otimizar seu ambiente, cobrindo categorias como otimização de custos e segurança. Ele pode alertar sobre **instâncias EC2 com baixa utilização**.
- **AWS Compute Optimizer:** Usa *machine learning* para analisar métricas de histórico de utilização e faz recomendações sobre os recursos computacionais ideais para suas cargas de trabalho, ajudando a reduzir custos e melhorar a performance, evitando o provisionamento excessivo.
- **Modelos de Precificação:**
  - **Sob Demanda (*On-demand*):** Você paga pelo uso sem compromisso de longo prazo, ideal para cargas de trabalho de curto prazo.
  - **Instâncias Reservadas:** Permitem reduzir custos (até 75%) se você tiver previsibilidade de uso por 1 ou 3 anos.
  - **Nível Gratuito:** Permite obter experiência prática com mais de 60 produtos. Inclui 12 meses grátis (como `750 horas` por mês de instâncias `t2.micro`/`t3.micro` e `5 GB` de armazenamento S3 Padrão) e ofertas que são sempre gratuitas (como Amazon DynamoDB).

## 3. Entendendo as Instâncias EC2 e Armazenamento

### Amazon Elastic Compute Cloud (EC2)

Uma instância do Amazon EC2 é um **servidor virtual** onde você pode escolher o sistema operacional e personalizá-lo com suas aplicações.

#### Imagens de Máquina da Amazon (AMI)

Uma **Amazon Machine Image (AMI)** é o modelo (template) de software usado para criar uma instância EC2. Pense nela como a "imagem" de um sistema operacional ou um "molde" que contém:

1.  Um Sistema Operacional (ex: Linux ou Windows).
2.  Servidores de aplicação (ex: Apache, Nginx).
3.  Aplicações e configurações de software.

Você pode usar AMIs fornecidas pela AWS, AMIs do AWS Marketplace (de terceiros) ou, o mais comum em ambientes corporativos, criar suas próprias AMIs personalizadas.

- **Personalização e *Golden Images*:** A principal aplicação de AMIs é a criação de uma ***"Golden Image"*** (Imagem Dourada). Você pode lançar uma instância básica, instalar todas as atualizações de segurança, configurar seus serviços, instalar seu software e, em seguida, criar uma AMI personalizada a partir dessa instância.
- **Velocidade e Escalabilidade:** Ao usar uma *Golden Image*, o tempo de inicialização de novas instâncias é drasticamente reduzido. Em vez de esperar 10 minutos para uma instância instalar patches e softwares, ela já sobe pronta em segundos. Isso é **essencial** para o funcionamento de **Grupos de Auto Scaling**, que precisam lançar novas instâncias rapidamente para lidar com picos de demanda.
- **Backup e Migração:** AMIs são, por baixo dos panos, armazenadas como *Snapshots* do EBS (que você menciona mais adiante). Você pode copiar uma AMI para outra Região da AWS para facilitar a migração de aplicações ou para criar planos de recuperação de desastres (Disaster Recovery).

- **Tipos de Instância:** O EC2 oferece uma ampla seleção de tipos de instância, que são combinações variadas de CPU, memória, armazenamento e capacidade de rede, otimizadas para diferentes casos de uso. Exemplos incluem:
  - **Uso Geral (`M-series`, `T-series`):** Oferecem um equilíbrio de recursos (CPU, memória, rede) e são boas para servidores web. As instâncias `T2`/`T3`/`T3a` são *expansíveis*, fornecendo uma linha de base de performance de CPU com capacidade de aumentar o uso quando necessário.
  - **Otimizadas para Computação (`C-series`):** Ideais para workloads com uso intenso de computação, como processamento em lote.
  - **Otimizadas para Memória (`R-series`):** Projetadas para fornecer performance rápida para workloads que processam grandes conjuntos de dados na memória, como grandes bancos de dados.
- **Otimização de Recursos:** A forma mais simples de reduzir custos em ambientes de desenvolvimento e teste é **desligar as instâncias** quando não estiverem sendo utilizadas.

### Armazenamento na Nuvem: EBS e S3

A AWS oferece serviços que cobrem três tipos principais de armazenamento: bloco, arquivo e objeto.

#### Amazon Elastic Block Store (EBS)

O EBS é a solução de **armazenamento em nível de bloco** da AWS, projetado para ser anexado a uma única instância EC2.

- **Funcionalidade:** Ele atua como um disco rígido físico, podendo ser formatado com um sistema de arquivos.
- **Persistência:** Os volumes EBS persistem independentemente do ciclo de vida da instância EC2.
- **Segurança:** Uma prática recomendada de segurança é **criptografar volumes e *snapshots*** do EBS.
- **Tipos de Volume:** Incluem:
  - **Uso Geral (SSD):** Recomendado como escolha padrão para uma ampla variedade de workloads, como bancos de dados de pequeno e médio porte.
  - **IOPS Provisionadas (SSD):** Projetado para alta performance consistente e baixa latência, ideal para aplicações com E/S intensa, como grandes bancos de dados.
- **Otimização de Custos com Snapshots:** Para volumes EBS que não estão mais ligados a uma instância ou estão subutilizados, a melhor prática é **tirar um *snapshot*** desse volume e, em seguida, **excluí-lo**, pois o armazenamento do *snapshot* tem custo inferior.

#### Amazon Simple Storage Service (S3)

O Amazon S3 é a solução de **armazenamento de objetos** totalmente gerenciada e altamente escalável da AWS.

- **Acesso:** Cada arquivo (objeto) é tratado de forma independente e pode ser acessado diretamente por meio de uma API. O S3 fornece 11 noves de durabilidade.
- **Classes de Armazenamento:** Existem diversas classes otimizadas para diferentes necessidades:
  - **S3 Standard:** Para objetos acessados com frequência.
  - **S3 Infrequent Access (IA):** Para objetos acessados com pouca frequência, mas que exigem acesso rápido quando necessário.
  - **S3 Glacier/Glacier Deep Archive:** Para arquivamento de objetos que não precisam de acesso imediato e serão mantidos por longos períodos.
 
  - ## 4. Diagrama de Arquitetura: Caso de Uso Simples

Para ilustrar como esses serviços interagem, vamos imaginar um cenário simples de **processamento de logs**.

### Caso de Uso: Processamento de Logs de Aplicação

Nesta arquitetura, temos uma aplicação (ex: um portal de notícias, um e-commerce) rodando em uma instância EC2. Esta aplicação gera arquivos de log que precisam ser analisados para encontrar erros.

1.  **EC2 (Servidor da Aplicação):** A instância EC2 está rodando a aplicação. Ela foi configurada para enviar (fazer upload) todos os seus arquivos de log (ex: `error.log`) periodicamente para um bucket S3.
2.  **S3 (Bucket de Logs Brutos):** Este bucket é o repositório central. Ele recebe os arquivos de log. Sua única função é armazenar.
3.  **Evento S3 -> Lambda:** O bucket S3 é configurado para disparar um "evento" (um gatilho) sempre que um novo arquivo (`PutObject`) é adicionado a ele.
4.  **Função Lambda (Processador):** Esse evento aciona uma Função Lambda. A Lambda executa um código que:
    a. Lê o arquivo de log que acabou de chegar.
    b. Filtra o conteúdo, procurando apenas por linhas que contenham a palavra "ERROR".
    c. Salva um novo arquivo, contendo apenas as linhas de erro, em um *outro* bucket S3.
5.  **S3 (Bucket de Logs Processados):** Este bucket armazena o resultado final e limpo, que pode ser usado por uma equipe de análise ou por um dashboard.

### Diagrama (Mermaid)

```mermaid
graph LR
    subgraph "Nuvem AWS"
        direction LR

        EC2(fa:fa-server Servidor Web EC2) -- "1. Envia logs" --> S3_Raw(fa:fa-archive Bucket S3 <br> Logs Brutos)
        
        S3_Raw -- "2. Dispara Evento (PutObject)" --> Lambda(fa:fa-bolt Função Lambda <br> Processador de Logs)
        
        Lambda -- "3. Salva logs filtrados" --> S3_Processed(fa:fa-archive Bucket S3 <br> Logs Processados)
    
    end
