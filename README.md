# aws-step-functions-lab
# Explorando Workflows Automatizados com AWS Step Functions

## Descrição do Desafio
Este repositório foi criado para documentar o desafio “Explorando Workflows Automatizados com AWS Step Functions” da Digital Innovation One (DIO).  
O objetivo é consolidar o aprendizado sobre orquestração de processos e automação de workflows na nuvem utilizando os serviços da AWS.

---

## Objetivos de Aprendizagem
Ao concluir este desafio, eu fui capaz de:
- Entender o conceito de State Machines e como elas controlam fluxos de execução;
- Compreender como o AWS Step Functions se integra a outros serviços da AWS (como Lambda, S3, DynamoDB e SNS);
- Criar e documentar workflows automatizados;
- Usar o GitHub para registrar aprendizados técnicos de forma organizada e reutilizável.

---

## O que é o AWS Step Functions
O AWS Step Functions é um serviço que permite criar fluxos de trabalho visuais para coordenar múltiplos serviços da AWS.  
Com ele, é possível automatizar processos complexos dividindo-os em etapas chamadas estados (states), que podem:
- Executar funções Lambda;
- Fazer decisões condicionais;
- Esperar intervalos de tempo;
- Executar etapas em paralelo;
- E muito mais.

Tudo isso sem a necessidade de código de orquestração manual — basta definir o fluxo em um arquivo JSON no formato Amazon States Language (ASL).

---

## Conceitos-Chave

| Conceito | Descrição |
|-----------|------------|
| State Machine | Representa todo o fluxo de execução (workflow) do Step Functions. |
| State (Estado) | Cada etapa do processo, como “Executar Lambda” ou “Decidir Caminho”. |
| Task | Estado responsável por executar uma função ou tarefa. |
| Choice | Estado de decisão (como um “if/else”). |
| Parallel | Permite executar múltiplos ramos ao mesmo tempo. |
| Catch / Retry | Controlam falhas e tentativas de reprocessamento. |
| Execution | Instância em execução do workflow. |

---

## Exemplo de Workflow

Exemplo simples de fluxo usando Step Functions:

```json
{
  "Comment": "Exemplo de State Machine simples",
  "StartAt": "ProcessarPedido",
  "States": {
    "ProcessarPedido": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:ProcessarPedido",
      "Next": "VerificarEstoque"
    },
    "VerificarEstoque": {
      "Type": "Choice",
      "Choices": [
        {
          "Variable": "$.estoqueDisponivel",
          "BooleanEquals": true,
          "Next": "ConfirmarPedido"
        }
      ],
      "Default": "NotificarFalha"
    },
    "ConfirmarPedido": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:ConfirmarPedido",
      "End": true
    },
    "NotificarFalha": {
      "Type": "Fail",
      "Error": "EstoqueIndisponivel",
      "Cause": "Produto esgotado."
    }
  }
}
