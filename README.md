# Explicação geral do Flow

## Overview

Este documento descreve o fluxo criado no LangFlow para gerar resumos consistentes e compactos de documentos longos, utilizando as técnicas de Map-Reduce e Few-Shot Learning. A abordagem foi desenvolvida para garantir que os resumos finais mantenham a essência do conteúdo original, ao mesmo tempo em que são reduzidos em tamanho.

## Objetivos

1. **Map-Reduce**: Utilizar a técnica de Map-Reduce para gerar resumos intermediários de diferentes partes do documento.
2. **Few-Shot Learning**: Aplicar Few-Shot Learning para consolidar os resumos intermediários em um resumo final compacto, garantindo alta similaridade com o conteúdo original.

## Estrutura do Fluxo

O fluxo é dividido em duas etapas principais:

### 1. Map-Reduce

Na primeira etapa, o documento completo é dividido em partes menores. Cada parte é processada individualmente para gerar um resumo intermediário. Esta abordagem permite lidar com documentos longos de forma eficiente, garantindo que cada seção seja adequadamente representada no resumo final.

**Passos:**
- Divisão do documento em partes menores.
- Processamento de cada parte para gerar resumos intermediários.
- Consolidação dos resumos intermediários.

### 2. Few-Shot Learning

Na segunda etapa, utilizamos Few-Shot Learning para forçar uma redução no tamanho do resumo final, mantendo a similaridade com o documento original. Esta técnica permite gerar resumos mais compactos sem perder informações cruciais.

**Passos:**
- Coleta dos resumos intermediários gerados na etapa anterior.
- Aplicação de Few-Shot Learning para consolidar e reduzir os resumos intermediários.
- Geração do resumo final compacto.

## Utilização

### Pré-requisitos

- LangFlow instalado e configurado.
- Modelos de Map-Reduce e Few-Shot Learning.

### Configuração

1. **Carregue o documento completo** no LangFlow.
2. **Configure a etapa de Map-Reduce**:
   - Divida o documento em partes menores.
   - Defina os parâmetros para gerar os resumos intermediários.
3. **Configure a etapa de Few-Shot Learning**:
   - Colete os resumos intermediários.
   - Defina os exemplos de Few-Shot para guiar a consolidação e redução do resumo final.

### Execução

1. **Execute a etapa de Map-Reduce** para gerar os resumos intermediários.
2. **Execute a etapa de Few-Shot Learning** para consolidar e reduzir os resumos intermediários em um resumo final compacto.
