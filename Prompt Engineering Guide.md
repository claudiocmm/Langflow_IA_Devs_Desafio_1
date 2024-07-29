# Prompt Engineering Guide

---

# **Introdução**

A engenharia de prompts é uma disciplina relativamente nova para desenvolver e otimizar prompts para usar eficientemente modelos de linguagem (LMs) para uma ampla variedade de aplicativos e tópicos de pesquisa. As habilidades imediatas de engenharia ajudam a entender melhor os recursos e as limitações dos modelos de linguagem grandes (LLMs). Os pesquisadores usam a engenharia de prompt para melhorar a capacidade dos LLMs em uma ampla gama de tarefas comuns e complexas, como resposta a perguntas e raciocínio aritmético. Os desenvolvedores usam engenharia de prompt para projetar técnicas de prompt robustas e eficazes que fazem interface com LLMs e outras ferramentas.

Este guia aborda os fundamentos dos prompts para fornecer uma ideia aproximada de como utiliza-los para interagir e instruir modelos de linguagem grandes (LLMs).

Todos os exemplos são testados com `text-davinci-003` (usando o playground do OpenAI), a menos que especificado de outra forma. Ele usa as configurações padrão, ou seja, `temperatura=0.7` e `top-p=1`.

# **Configurações LLM**

Ao trabalhar com prompts, você estará interagindo com o LLM diretamente ou por meio de uma API. Você pode configurar alguns parâmetros para obter resultados diferentes para seus prompts.

**Temperatura** - Resumindo, quanto menor a `temperatura`, mais determinísticos são os resultados, no sentido de que o próximo token provável mais alto é sempre escolhido. O aumento da temperatura pode levar a mais aleatoriedade, incentivando saídas mais diversificadas ou criativas. Estamos essencialmente aumentando os pesos dos outros tokens possíveis. Em termos de aplicação, podemos querer usar um valor de temperatura mais baixo para tarefas como controle de qualidade baseado em fatos encorajando respostas mais factuais e concisas. Para geração de poemas ou outras tarefas criativas, pode ser benéfico aumentar o valor da temperatura.

**Top_p** - Da mesma forma, com o `top_p`, uma técnica de amostragem com temperatura chamada amostragem de núcleo, você pode controlar o grau de determinismo do modelo na geração de uma resposta. Se você está procurando respostas exatas e factuais, mantenha isso baixo. Se você estiver procurando respostas mais diversificadas, aumente para um valor mais alto.

A recomendação geral é alterar um, não ambos.

Antes de começar com alguns exemplos básicos, lembre-se de que seus resultados podem variar dependendo da versão do LLM que você está usando.

# **Noções Básicas de Prompt**

## **Prompts Básicos**

Você pode conseguir muito com prompts simples, mas a qualidade dos resultados depende da quantidade de informações que você fornece e de quão bem elaboradas são. Um prompt pode conter informações como *instrução* ou *pergunta* que você está passando para o modelo e incluir outros detalhes como *contexto*, *entradas* ou *exemplos*. Você pode usar esses elementos para instruir melhor o modelo e, como resultado, obter melhores resultados.

Vamos começar analisando um exemplo básico de um prompt simples:

*Prompt*

```
O céu é
```

*Saída:*

```
azulO céu é azul em um dia claro. Em um dia nublado, o céu pode ser cinza ou branco.
```

Como você pode ver, o modelo de linguagem gera uma continuação de strings que fazem sentido no contexto `"O céu é"`. A saída pode ser inesperada ou distante da tarefa que queremos realizar.

Este exemplo básico também destaca a necessidade de fornecer mais contexto ou instruções sobre o que especificamente queremos alcançar.

Vamos tentar melhorar um pouco:

*Prompt:*

```
Complete a sentença:O céu é
```

*Saída:*

```
tão lindo.
```

Isto é melhor? Bem, dissemos ao modelo para completar a frase para que o resultado fique muito melhor, pois segue exatamente o que dissemos para fazer ("complete a frase"). Essa abordagem de projetar prompts ideais para instruir o modelo a executar uma tarefa é chamada de **engenharia de prompt**.

O exemplo acima é uma ilustração básica do que é possível com LLMs hoje. Os LLMs de hoje são capazes de executar todos os tipos de tarefas avançadas que variam de resumo de texto a raciocínio matemático e geração de código.

## **Formatação de prompt**

Tentamos um prompt muito simples acima. Um prompt padrão tem o seguinte formato:

```
<Pergunta>?
```

ou

```
<Instrução>
```

Isso pode ser formatado em um formato de resposta a perguntas (QA), que é padrão em muitos conjuntos de dados de QA, como segue:

```
Q: <Pergunta>?A:
```

Ao solicitar como o acima, também chamado de *prompt de tiro zero*, ou seja, você está solicitando diretamente ao modelo uma resposta sem nenhum exemplo ou demonstração sobre a tarefa que deseja realizar. Alguns modelos de linguagem grandes têm a capacidade de executar prompts zero-shot, mas isso depende da complexidade e do conhecimento da tarefa em questão.

Dado o formato padrão acima, uma técnica popular e eficaz para solicitação é chamada de *prompt de poucos tiros*, onde fornecemos exemplos (ou seja, demonstrações). Os prompts de poucos tiros podem ser formatados da seguinte maneira:

```
<Pergunta>?<Resposta><Pergunta>?<Resposta><Pergunta>?<Resposta><Pergunta>?
```

A versão do formato QA ficaria assim:

```
Q: <Pergunta>?A: <Resposta>Q: <Pergunta>?A: <Resposta>Q: <Pergunta>?A: <Resposta>Q: <Pergunta>?A:
```

Lembre-se de que não é necessário usar o formato QA. O formato do prompt depende da tarefa em mãos. Por exemplo, você pode executar uma tarefa de classificação simples e fornecer exemplares que demonstrem a tarefa da seguinte forma:

*Prompt:*

```
Isso é incrível! // PositivoIsto é mau! // NegativoUau, esse filme foi radical! // PositivoQue espetáculo horrível! //
```

*Saída:*

```
Negativo
```

Os prompts de poucos tiros permitem o aprendizado no contexto, que é a capacidade dos modelos de linguagem de aprender tarefas dadas algumas demonstrações.

# **Elementos de um prompt**

À medida que abordamos mais e mais exemplos e aplicativos possíveis com a engenharia de prompt, você notará que existem certos elementos que compõem um prompt.

Um prompt pode conter qualquer um dos seguintes componentes:

**Instrução** - uma tarefa ou instrução específica que você deseja que o modelo execute

**Contexto** - pode envolver informações externas ou contexto adicional que pode direcionar o modelo para melhores respostas

**Dados de entrada** - é a entrada ou pergunta para a qual estamos interessados em encontrar uma resposta

**Indicador de saída** - indica o tipo ou formato da saída.

Nem todos os componentes são necessários para um prompt e o formato depende da tarefa em questão. Abordaremos exemplos mais concretos nos próximos guias.

# **Dicas gerais para projetar prompts**

Aqui estão algumas dicas para manter em mente ao projetar seus prompts:

### **Comece Simples**

Ao começar a criar prompts, você deve ter em mente que é realmente um processo iterativo que requer muita experimentação para obter os melhores resultados. Usar um playground simples como OpenAI ou Cohere's é um bom ponto de partida.

Você pode começar com prompts simples e continuar adicionando mais elementos e contexto à medida que busca melhores resultados. O controle de versão do seu prompt ao longo do caminho é vital por esse motivo. Ao ler o guia, você verá muitos exemplos em que a especificidade, a simplicidade e a concisão geralmente lhe darão melhores resultados.

Quando você tem uma grande tarefa que envolve muitas subtarefas diferentes, pode tentar dividir a tarefa em subtarefas mais simples e continuar aumentando conforme obtém melhores resultados. Isso evita adicionar muita complexidade ao processo de design do prompt no início.

### **A instrução**

Você pode criar prompts eficazes para várias tarefas simples usando comandos para instruir o modelo sobre o que deseja alcançar, como "Escrever", "Classificar", "Resumir", "Traduzir", "Ordenar" etc.

Tenha em mente que você também precisa experimentar muito para ver o que funciona melhor. Experimente instruções diferentes com palavras-chave, contextos e dados diferentes e veja o que funciona melhor para seu caso de uso e tarefa específicos. Normalmente, quanto mais específico e relevante for o contexto para a tarefa que você está tentando executar, melhor. Abordaremos a importância da amostragem e da adição de mais contexto nos próximos guias.

Outros recomendam que as instruções sejam colocadas no início do prompt. Também é recomendado que algum separador claro como "###" seja usado para separar a instrução e o contexto.

Por exemplo:

*Prompt:*

```
### Instrução ###Traduza o texto abaixo para o espanhol:Texto: "olá!"
```

*Saída:*

```
¡Hola!
```

### **Especificidade**

Seja muito específico sobre a instrução e a tarefa que deseja que o modelo execute. Quanto mais descritivo e detalhado for o prompt, melhores serão os resultados. Isso é particularmente importante quando você tem um resultado desejado ou estilo de geração que está buscando. Não há tokens ou palavras-chave específicas que levem a melhores resultados. É mais importante ter um bom formato e um prompt descritivo. Na verdade, fornecer exemplos no prompt é muito eficaz para obter a saída desejada em formatos específicos.

Ao criar prompts, você também deve ter em mente o tamanho do prompt, pois há limitações em relação a quão grande ele pode ser. Pensar em quão específico e detalhado você deve ser é algo a se considerar. Incluir muitos detalhes desnecessários não é necessariamente uma boa abordagem. Os detalhes devem ser relevantes e contribuir para a tarefa em mãos. Isso é algo que você precisará experimentar muito. Incentivamos muita experimentação e iteração para otimizar os prompts de seus aplicativos.

Como exemplo, vamos tentar um prompt simples para extrair informações específicas de um texto.

*Prompt:*

```
Extraia o nome dos lugares no texto a seguir.Formato desejado:Local: <lista_de_nomes_de_empresa_separados_por_vírgula>Input: "Embora estes desenvolvimentos sejam encorajadores para os investigadores, muito ainda é um mistério. “Muitas vezes temos uma caixa preta entre o cérebro e o efeito que vemos na periferia”, diz Henrique Veiga-Fernandes, neuroimunologista do Centro Champalimaud para o Desconhecido em Lisboa. “Se queremos utilizá-lo no contexto terapêutico, precisamos de facto de perceber o mecanismo."
```

*Saída:*

```
Local: Centro Champalimaud para o Desconhecido, Lisboa
```

O texto de entrada é obtido [neste artigo da Nature](https://www.nature.com/articles/d41586-023-00509-z).

### **Evite Imprecisões**

Dadas as dicas acima sobre como ser detalhado e melhorar o formato, é fácil cair na armadilha de querer ser muito inteligente sobre os prompts e potencialmente criar descrições imprecisas. Geralmente é melhor ser específico e direto. A analogia aqui é muito semelhante à comunicação eficaz - quanto mais direta, mais eficaz a mensagem é transmitida.

Por exemplo, você pode estar interessado em aprender o conceito de engenharia de prompt. Você pode tentar algo como:

```
Explique o conceito de engenharia de prompt. Mantenha a explicação curta, apenas algumas frases, e não seja muito descritivo.
```

Não está claro no prompt acima quantas frases usar e qual estilo. Você ainda pode obter uma boa resposta com o prompt acima, mas o melhor prompt seria aquele que é muito específico, conciso e direto ao ponto. Algo como:

```
Use 2 a 3 frases para explicar o conceito de engenharia de prompt a um aluno do ensino médio.
```

### **Fazer ou não fazer?**

Outra dica comum ao criar prompts é evitar dizer o que não fazer, mas dizer o que fazer. Isso incentiva mais especificidade e concentra-se nos detalhes que levam a boas respostas do modelo.

Aqui está um exemplo de um chatbot de recomendação de filme falhando exatamente no que eu não quero que ele faça por causa de como escrevi a instrução -- focando no que não fazer.

*Prompt:*

```
O agente a seguir recomenda filmes para um cliente. NÃO PEÇA INTERESSES. NÃO PEÇA INFORMAÇÕES PESSOAIS.Cliente: Por favor, recomende um filme baseado nos meus interesses.Agente:
```

*Saída:*

```
Claro, posso recomendar um filme baseado em seus interesses. Que tipo de filme você gostaria de assistir? Você prefere ação, comédia, romance ou outra coisa?
```

Aqui está um prompt melhor:

*Prompt:*

```
O agente a seguir recomenda filmes para um cliente. O agente é responsável por recomendar um filme dos principais filmes de tendências globais. Deve abster-se de perguntar aos usuários sobre suas preferências e evitar pedir informações pessoais. Se o agente não tiver um filme para recomendar, ele deve responder "Desculpe, não foi possível encontrar um filme para recomendar hoje.".Cliente: Por favor, recomende um filme baseado nos meus interesses.Agente:
```

*Saída:*

```
Desculpe, não tenho nenhuma informação sobre seus interesses. No entanto, aqui está uma lista dos principais filmes de tendências globais no momento: [lista de filmes]. Espero que encontre algo que goste!
```

Alguns dos exemplos acima foram adotados do [artigo "Melhores práticas para engenharia de prompt com OpenAI API".](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api)

# **Exemplos de Prompts**

Na seção anterior, apresentamos e fornecemos exemplos básicos de como solicitar LLMs.

Nesta seção, forneceremos mais exemplos de como os prompts são usados para realizar diferentes tarefas e introduziremos os principais conceitos ao longo do caminho. Muitas vezes, a melhor maneira de aprender conceitos é através de exemplos. Abaixo, abordamos alguns exemplos de como prompts bem elaborados podem ser usados para executar diferentes tipos de tarefas.

Tópicos:

- [Resumo de Texto](https://www.promptingguide.ai/pt/introduction/examples#resumo-de-texto)
- [Extração de informações](https://www.promptingguide.ai/pt/introduction/examples#extra%C3%A7%C3%A3o-de-informa%C3%A7%C3%B5es)
- [Resposta à pergunta](https://www.promptingguide.ai/pt/introduction/examples#resposta-a-pergunta)
- [Classificação de texto](https://www.promptingguide.ai/pt/introduction/examples#classifica%C3%A7%C3%A3o-de-texto)
- [Conversação](https://www.promptingguide.ai/pt/introduction/examples#conversa%C3%A7%C3%A3o)
- [Geração de código](https://www.promptingguide.ai/pt/introduction/examples#gera%C3%A7%C3%A3o-de-c%C3%B3digo)
- [Raciocínio](https://www.promptingguide.ai/pt/introduction/examples#racioc%C3%ADnio)

---

## **Resumo de texto**

Uma das tarefas padrão na geração de linguagem natural é o resumo de texto. A sumarização de texto pode incluir preferências e domínios diferentes. Na verdade, uma das aplicações mais promissoras dos modelos de linguagem é a capacidade de resumir artigos e conceitos em resumos rápidos e fáceis de ler. Vamos tentar uma tarefa básica de resumo usando prompts.

Digamos que estou interessado em aprender sobre antibióticos, poderia tentar um prompt como este:

*Prompt:*

```
Explique os antibióticosA:
```

*Saída:*

```
Os antibióticos são um tipo de medicamento usado para tratar infecções bacterianas. Eles funcionam matando as bactérias ou impedindo-as de se reproduzir, permitindo que o sistema imunológico do corpo combata a infecção. Os antibióticos são geralmente tomados por via oral na forma de comprimidos, cápsulas ou soluções líquidas, ou às vezes administrados por via intravenosa. Eles não são eficazes contra infecções virais e usá-los de forma inadequada pode levar à resistência aos antibióticos.
```

O "A:" é um formato de prompt explícito usado para responder perguntas. Eu usei isso aqui para dizer ao modelo que há um futuro esperado. Neste exemplo, não está claro como isso é útil ou não, mas deixaremos isso para exemplos posteriores. Vamos apenas supor que isso é muita informação e queremos resumi-la ainda mais. Na verdade, podemos instruir o modelo a resumir em uma frase da seguinte forma:

*Prompt:*

```
Os antibióticos são um tipo de medicamento usado para tratar infecções bacterianas. Eles funcionam matando as bactérias ou impedindo-as de se reproduzir, permitindo que o sistema imunológico do corpo combata a infecção. Os antibióticos são geralmente tomados por via oral na forma de comprimidos, cápsulas ou soluções líquidas, ou às vezes administrados por via intravenosa. Eles não são eficazes contra infecções virais e usá-los de forma inadequada pode levar à resistência aos antibióticos.**Explique o que foi dito acima em uma frase**:
```

*Saída:*

```
Os antibióticos são medicamentos usados para tratar infecções bacterianas, matando as bactérias ou impedindo-as de se reproduzir, mas não são eficazes contra vírus e o uso excessivo pode levar à resistência aos antibióticos.
```

Sem prestar muita atenção à precisão da saída acima, algo que abordaremos em um guia posterior, o modelo tentou resumir o parágrafo em uma frase. Você pode ficar esperto com as instruções, mas deixaremos isso para um capítulo posterior. Sinta-se à vontade para fazer uma pausa aqui e experimentar para ver se obtém melhores resultados.

---

## **Extração de Informações**

Embora os modelos de linguagem sejam treinados para executar a geração de linguagem natural e tarefas relacionadas, eles também são muito capazes de realizar classificação e uma série de outras tarefas de processamento de linguagem natural (NLP).

Aqui está um exemplo de um prompt que extrai informações de um determinado parágrafo.

*Prompt:*

```
As declarações de contribuição do autor e os agradecimentos em trabalhos de pesquisa devem indicar clara e especificamente se, e em que medida, os autores usaram tecnologias de IA, como ChatGPT, na preparação de seus manuscritos e análises. Eles também devem indicar quais LLMs foram usados. Isso alertará os editores e revisores para examinar os manuscritos com mais cuidado em busca de possíveis vieses, imprecisões e créditos de origem impróprios. Da mesma forma, os periódicos científicos devem ser transparentes sobre o uso de LLMs, por exemplo, ao selecionar manuscritos enviados.**Mencione o produto baseado em modelo de linguagem grande mencionado no parágrafo acima**:
```

*Saída:*

```
O produto baseado em modelo de linguagem grande mencionado no parágrafo acima é o ChatGPT.
```

Existem muitas maneiras de melhorar os resultados acima, mas isso já é muito útil.

Até agora deve ser óbvio que você pode pedir ao modelo para executar diferentes tarefas simplesmente instruindo-o sobre o que fazer. Esse é um recurso poderoso que os desenvolvedores de produtos de IA já estão usando para criar produtos e experiências poderosos.

Fonte do parágrafo: [ChatGPT: cinco prioridades para pesquisa](https://www.nature.com/articles/d41586-023-00288-7)

---

## **Resposta a perguntas**

Uma das melhores maneiras de fazer com que o modelo responda a respostas específicas é melhorar o formato do prompt. Conforme abordado anteriormente, um prompt pode combinar instruções, contexto, entrada e indicadores de saída para obter melhores resultados.

Embora esses componentes não sejam necessários, torna-se uma boa prática, pois quanto mais específico você for com a instrução, melhores resultados obterá. Abaixo está um exemplo de como isso ficaria seguindo um prompt mais estruturado.

*Prompt:*

```
Responda a pergunta com base no contexto abaixo. Mantenha a resposta curta e concisa. Responda "Não tenho certeza sobre a resposta" se não tiver certeza da resposta.Contexto: Teplizumab tem suas raízes em uma empresa farmacêutica de Nova Jersey chamada Ortho Pharmaceutical. Lá, os cientistas geraram uma versão inicial do anticorpo, apelidada de OKT3. Originalmente proveniente de camundongos, a molécula foi capaz de se ligar à superfície das células T e limitar seu potencial de morte celular. Em 1986, foi aprovado para ajudar a prevenir a rejeição de órgãos após transplantes renais, tornando-se o primeiro anticorpo terapêutico permitido para uso humano.Pergunta: De onde veio originalmente o OKT3?Responder:
```

*Saída:*

```
Camundongos.
```

Contexto obtido da [Nature](https://www.nature.com/articles/d41586-023-00400-x).

---

## **Classificação de texto**

Até agora, usamos instruções simples para executar uma tarefa. Como um engenheiro de prompt, você precisará melhorar o fornecimento de melhores instruções. Mas isso não é tudo! Você também descobrirá que, para casos de uso mais difíceis, apenas fornecer instruções não será suficiente. É aqui que você precisa pensar mais sobre o contexto e os diferentes elementos que pode usar em um prompt. Outros elementos que você pode fornecer são `input data` ou `examples`.

Vamos tentar demonstrar isso fornecendo um exemplo de classificação de texto.

*Prompt:*

```
Classifique o texto em neutro, negativo ou positivo.Texto: Acho que a comida estava boa.Sentimento:
```

*Saída:*

```
Neutro
```

Demos a instrução para classificar o texto e o modelo respondeu com `'Neutro'` que está correto. Não há nada de errado nisso, mas digamos que o que realmente precisamos é que o modelo dê o rótulo no formato exato que queremos. Portanto, em vez de `Neutral`, queremos que retorne `neutral`. Como alcançamos isso? Existem diferentes maneiras de fazer isso. Nós nos preocupamos com a especificidade aqui, portanto, quanto mais informações pudermos fornecer, melhores serão os resultados. Podemos tentar fornecer exemplos para especificar o comportamento correto. Vamos tentar de novo:

*Prompt:*

```
Classifique o texto em neutro, negativo ou positivo.Texto: Acho que as férias estão boas.Sentimento: neutroTexto: Acho que a comida estava boa.Sentimento:
```

*Saída:*

```
neutro
```

Perfeito! Desta vez, o modelo retornou `neutro`, que é o rótulo específico que eu estava procurando. Parece que o exemplo fornecido no prompt ajudou o modelo a ser específico em sua saída. Para destacar por que às vezes ser específico é importante, confira este exemplo e identifique o problema:

*Prompt:*

```
Classifique o texto em neutro, negativo ou positivo.Texto: Acho que as férias estão boas.Sentimento:
```

*Saída:*

```
Neutro
```

Qual é o problema aqui?

---

## **Conversação**

Talvez uma das coisas mais interessantes que você pode conseguir com a engenharia imediata seja instruir o sistema LLM sobre como se comportar, sua intenção e sua identidade. Isso é particularmente útil quando você está construindo sistemas de conversação, como chatbots de atendimento ao cliente.

Por exemplo, vamos criar um sistema de conversação capaz de gerar respostas mais técnicas e científicas às perguntas. Observe como estamos dizendo explicitamente como se comportar por meio da instrução. Às vezes, isso é chamado de *solicitação de função*.

*Prompt:*

```
A seguir, uma conversa com um assistente de pesquisa de IA. O tom assistente é técnico e científico.Humano: Olá, quem é você?AI: Saudações! Eu sou um assistente de pesquisa de IA. Como posso te ajudar hoje?Humano: Você pode me falar sobre a criação de buracos negros?IA:
```

*Saída:*

```
Claro! Buracos negros são regiões do espaço-tempo onde a força gravitacional é tão forte que nada, nem mesmo a luz, consegue escapar dela. Eles são criados quando uma estrela muito massiva morre e seu núcleo colapsa sobre si mesmo, formando uma singularidade de densidade infinita. A intensa gravidade da singularidade atrai toda a matéria e radiação ao seu redor, criando o buraco negro.
```

Nosso assistente de pesquisa de IA parece um pouco técnico demais, certo? Ok, vamos mudar esse comportamento e instruir o sistema a dar respostas mais acessíveis.

*Prompt:*

```
A seguir, uma conversa com um assistente de pesquisa de IA. As respostas do assistente devem ser fáceis de entender mesmo por alunos do ensino fundamental.Humano: Olá, quem é você?AI: Saudações! Eu sou um assistente de pesquisa de IA. Como posso te ajudar hoje?Humano: Você pode me falar sobre a criação de buracos negros?IA:
```

*Saída:*

```
Claro! Os buracos negros são criados quando uma estrela muito massiva fica sem combustível e colapsa sobre si mesma. A gravidade da estrela é tão forte que nada, nem mesmo a luz, pode escapar de sua atração. Isso cria um ponto no espaço-tempo com uma intensa força gravitacional chamada buraco negro.
```

Acho que fizemos algum progresso. Você pode continuar melhorando. Tenho certeza que se você adicionar mais exemplos você pode obter resultados ainda melhores.

---

## **Geração de Código**

Uma aplicação em que os LLMs são bastante eficazes é a geração de código. O Copilot é um ótimo exemplo disso. Há um grande número de tarefas de geração de código que você pode executar com prompts inteligentes. Vejamos alguns exemplos abaixo.

Primeiro, vamos tentar um programa simples que saúda o usuário.

*Prompt:*

```
/*Pergunte ao usuário o nome dele e diga "Olá"*/
```

*Saída:*

```
let name = prompt("Qual é o seu nome?");console.log(`Olá, ${name}!`);
```

Você pode ver que nem precisamos especificar a linguagem a ser usada.

Vamos mudar um pouco os níveis. Quero mostrar a você como os LLMs podem ser poderosos com um pouco mais de esforço no design dos prompts.

*Prompt:*

```
"""Tabela departamentos, colunas = [DepartmentId, DepartmentName]Alunos da tabela, colunas = [DepartmentId, StudentId, StudentName]Crie uma consulta MySQL para todos os alunos do Departamento de Ciência da Computação"""
```

*Saída:*

```
SELECT AlunoId, AlunoNomeDE alunosWHERE DepartmentId IN (SELECT DepartmentId FROM departamentos WHERE DepartmentName = 'Ciência da Computação');
```

Isso é muito impressionante. Nesse caso, fornecemos dados sobre o esquema do banco de dados e solicitamos que ele gerasse uma consulta MySQL válida.

---

## **Raciocínio**

Talvez uma das tarefas mais difíceis para um LLM hoje seja aquela que requer alguma forma de raciocínio. O raciocínio é uma das áreas que mais me entusiasma devido aos tipos de aplicativos complexos que podem surgir dos LLMs.

Houve algumas melhorias em tarefas envolvendo capacidades matemáticas. Dito isso, é importante observar que os LLMs atuais lutam para executar tarefas de raciocínio, portanto, isso requer técnicas de engenharia de prompt ainda mais avançadas. Abordaremos essas técnicas avançadas no próximo guia. Por enquanto, abordaremos alguns exemplos básicos para mostrar as capacidades aritméticas.

*Prompt:*

```
Quanto é 9.000 * 9.000?
```

*Saída:*

```
81.000.000
```

Vamos tentar algo mais difícil.

*Prompt:*

```
Os números ímpares neste grupo somam um número par: 15, 32, 5, 13, 82, 7, 1.A:
```

*Saída*

```
Não, os números ímpares neste grupo somam um número ímpar: 119.
```

Isso é incorreto! Vamos tentar melhorar isso melhorando o prompt.

*Prompt:*

```
Os números ímpares neste grupo somam um número par: 15, 32, 5, 13, 82, 7, 1.Resolva dividindo o problema em etapas. Primeiro, identifique os números ímpares, some-os e indique se o resultado é par ou ímpar.
```

*Saída:*

```
Números ímpares: 15, 5, 13, 7, 1Total 4141 é um número ímpar.
```

Muito melhor, certo? A propósito, tentei isso algumas vezes e o sistema às vezes falha. Se você fornecer instruções melhores combinadas com exemplos, isso pode ajudar a obter resultados mais precisos.

Continuaremos a incluir mais exemplos de aplicativos comuns nesta seção do guia.

Na próxima seção, abordaremos conceitos e técnicas de engenharia de prompt ainda mais avançados para melhorar o desempenho em todas essas e em tarefas mais difíceis.

# **Zero-Shot Prompting**

Os LLMs hoje treinados em grandes quantidades de dados e sintonizados para seguir instruções são capazes de executar tarefas de tiro zero. Tentamos alguns exemplos de tiro zero na seção anterior. Aqui está um dos exemplos que usamos:

*Prompt:*

```
Classifique o texto em neutro, negativo ou positivo.Texto: Acho que as férias estão boas.Sentimento:
```

*Saída:*

```
Neutro
```

Observe que no prompt acima não fornecemos nenhum exemplo ao modelo -- esses são os recursos de tiro zero em ação.

O ajuste de instrução demonstrou melhorar o aprendizado de tiro zero [Wei et al. (2022)](https://arxiv.org/pdf/2109.01652.pdf). O ajuste de instrução é essencialmente o conceito de modelos de ajuste fino em conjuntos de dados descritos por meio de instruções. Além disso, [RLHF](https://arxiv.org/abs/1706.03741) (aprendizado por reforço a partir de feedback humano) foi adotado para escalar o ajuste de instruções em que o modelo é alinhado para melhor atender às preferências humanas. Este desenvolvimento recente alimenta modelos como o ChatGPT. Discutiremos todas essas abordagens e métodos nas próximas seções.

Quando o tiro zero não funciona, é recomendável fornecer demonstrações ou exemplos no prompt que levam ao prompt de poucos tiros. Na próxima seção, demonstramos a solicitação de poucos disparos.

# **Few-Shot Prompting**

Embora os modelos de linguagem grande demonstrem recursos notáveis de disparo zero, eles ainda ficam aquém em tarefas mais complexas ao usar a configuração de disparo zero. O prompt de poucos disparos pode ser usado como uma técnica para permitir o aprendizado no contexto, onde fornecemos demonstrações no prompt para direcionar o modelo para um melhor desempenho. As demonstrações servem de condicionamento para exemplos subsequentes onde gostaríamos que o modelo gerasse uma resposta.

De acordo com [Touvron et al. 2023](https://arxiv.org/pdf/2302.13971.pdf) poucas propriedades de tiro apareceram pela primeira vez quando os modelos foram dimensionados para um tamanho suficiente [(Kaplan et al., 2020)](https://arxiv.org/abs/2001.08361).

Vamos demonstrar a solicitação de poucos disparos por meio de um exemplo apresentado em [Brown et al. 2020](https://arxiv.org/abs/2005.14165). No exemplo, a tarefa é usar corretamente uma nova palavra em uma frase.

*Prompt:*

```
Um "whatpu" é um pequeno animal peludo nativo da Tanzânia. Exemplo de frase que usaa palavra whatpu é:Estávamos viajando pela África e vimos esses whatpus muito fofos."Farduddlear" significa pular para cima e para baixo muito rápido. Exemplo de frase que usaa palavra farduddlear é:
```

*Saída:*

```
Quando ganhamos o jogo, todos farduddleamos em festejo.
```

Podemos observar que o modelo aprendeu de alguma forma como executar a tarefa fornecendo apenas um exemplo (ou seja, 1-shot). Para tarefas mais difíceis, podemos experimentar aumentar as demonstrações (por exemplo, 3 tiros, 5 tiros, 10 tiros, etc.).

Seguindo as descobertas de [Min et al. (2022)](https://arxiv.org/abs/2202.12837), aqui estão mais algumas dicas sobre demonstrações/exemplares ao fazer poucos disparos:

- "o espaço do rótulo e a distribuição do texto de entrada especificado pelas demonstrações são importantes (independentemente de os rótulos estarem corretos para entradas individuais)"
- o formato que você usa também desempenha um papel fundamental no desempenho, mesmo que você use apenas rótulos aleatórios, isso é muito melhor do que nenhum rótulo.
- resultados adicionais mostram que selecionar rótulos aleatórios de uma distribuição verdadeira de rótulos (em vez de uma distribuição uniforme) também ajuda.

Vamos experimentar alguns exemplos. Vamos primeiro tentar um exemplo com rótulos aleatórios (o que significa que os rótulos Negativo e Positivo são atribuídos aleatoriamente às entradas):

*Prompt:*

```
Isso é incrível! // NegativoIsto é mau! // PositivoUau, esse filme foi rad! // PositivoQue espetáculo horrível! //
```

*Saída:*

```
Negativo
```

Ainda obtemos a resposta correta, mesmo que os rótulos tenham sido randomizados. Observe que também mantivemos o formato, o que também ajuda. Na verdade, com mais experimentação, parece que os modelos GPT mais recentes que estamos experimentando estão se tornando mais robustos até mesmo para formatos aleatórios. Exemplo:

*Prompt:*

```
Positivo Isso é incrível!Isto é mau! NegativoUau, esse filme foi rad!PositivoQue espetáculo horrível! --
```

*Saída:*

```
Negativo
```

Não há consistência no formato acima, mas o modelo ainda previu o rótulo correto. Temos que realizar uma análise mais completa para confirmar se isso vale para tarefas diferentes e mais complexas, incluindo diferentes variações de prompts.

### **Limitações da solicitação de poucos disparos**

O prompt padrão de poucos disparos funciona bem para muitas tarefas, mas ainda não é uma técnica perfeita, especialmente ao lidar com tarefas de raciocínio mais complexas. Vamos demonstrar por que esse é o caso. Você se lembra do exemplo anterior onde fornecemos a seguinte tarefa:

```
Os números ímpares neste grupo somam um número par: 15, 32, 5, 13, 82, 7, 1.A:
```

Se tentarmos isso novamente, o modelo produzirá o seguinte:

```
Sim, os números ímpares neste grupo somam 107, que é um número par.
```

Esta não é a resposta correta, o que não apenas destaca as limitações desses sistemas, mas também a necessidade de uma engenharia imediata mais avançada.

Vamos tentar adicionar alguns exemplos para ver se a solicitação de poucos tiros melhora os resultados.

*Prompt:*

```
Os números ímpares neste grupo somam um número par: 4, 8, 9, 15, 12, 2, 1.A: A resposta é Falsa.Os números ímpares neste grupo somam um número par: 17, 10, 19, 4, 8, 12, 24.A: A resposta é Verdadeira.Os números ímpares neste grupo somam um número par: 16, 11, 14, 4, 8, 13, 24.A: A resposta é Verdadeira.Os números ímpares neste grupo somam um número par: 17, 9, 10, 12, 13, 4, 2.A: A resposta é Falsa.Os números ímpares neste grupo somam um número par: 15, 32, 5, 13, 82, 7, 1.A:
```

*Saída:*

```
A resposta é verdadeira.
```

Isso não funcionou. Parece que a solicitação de poucos disparos não é suficiente para obter respostas confiáveis para esse tipo de problema de raciocínio. O exemplo acima fornece informações básicas sobre a tarefa. Se você olhar mais de perto, o tipo de tarefa que introduzimos envolve mais algumas etapas de raciocínio. Em outras palavras, pode ajudar se dividirmos o problema em etapas e demonstrarmos isso ao modelo. Mais recentemente, [inserção de cadeia de pensamento (CoT)](https://arxiv.org/abs/2201.11903) foi popularizada para abordar mais aritmética complexa, senso comum e tarefas de raciocínio simbólico.

No geral, parece que fornecer exemplos é útil para resolver algumas tarefas. Quando a solicitação de disparo zero e a solicitação de poucos disparos não são suficientes, isso pode significar que tudo o que foi aprendido pelo modelo não é suficiente para se sair bem na tarefa. A partir daqui, é recomendável começar a pensar em ajustar seus modelos ou experimentar técnicas de solicitação mais avançadas. A seguir, falaremos sobre uma das técnicas populares de sugestão, chamada de sugestão em cadeia de pensamento, que ganhou muita popularidade.

# **Cadeia-de-Pensamento Prompt**

## **Cadeia-de-Pensamento (CoT) Prompting**

[https://www.promptingguide.ai/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fcot.1933d9fe.png&w=1920&q=75](https://www.promptingguide.ai/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fcot.1933d9fe.png&w=1920&q=75)

Fonte da imagem: [Wei et al. (2022)](https://arxiv.org/abs/2201.11903)

Introduzido em [Wei et al. (2022)](https://arxiv.org/abs/2201.11903), a solicitação de cadeia de pensamento (CoT) permite recursos de raciocínio complexos por meio de etapas intermediárias de raciocínio. Você pode combiná-lo com prompts de poucos tiros para obter melhores resultados em tarefas mais complexas que exigem raciocínio antes de responder.

*Prompt:*

```
Os números ímpares neste grupo somam um número par: 4, 8, 9, 15, 12, 2, 1.R: Somando todos os números ímpares (9, 15, 1) dá 25. A resposta é Falso.Os números ímpares neste grupo somam um número par: 17, 10, 19, 4, 8, 12, 24.R: Somando todos os números ímpares (17, 19) dá 36. A resposta é Verdadeiro.Os números ímpares neste grupo somam um número par: 16, 11, 14, 4, 8, 13, 24.R: Somando todos os números ímpares (11, 13) dá 24. A resposta é Verdadeiro.Os números ímpares neste grupo somam um número par: 17, 9, 10, 12, 13, 4, 2.R: Somando todos os números ímpares (17, 9, 13) dá 39. A resposta é Falso.Os números ímpares neste grupo somam um número par: 15, 32, 5, 13, 82, 7, 1.A:
```

*Saída:*

```
Somando todos os números ímpares (15, 5, 13, 7, 1) dá 41. A resposta é Falso.
```

Uau! Podemos ver um resultado perfeito quando fornecemos a etapa de raciocínio. Na verdade, podemos resolver essa tarefa fornecendo ainda menos exemplos, ou seja, apenas um exemplo parece suficiente:

*Prompt:*

```
Os números ímpares neste grupo somam um número par: 4, 8, 9, 15, 12, 2, 1.R: Somando todos os números ímpares (9, 15, 1) dá 25. A resposta é Falso.Os números ímpares neste grupo somam um número par: 15, 32, 5, 13, 82, 7, 1.A:
```

*Saída:*

```
Somando todos os números ímpares (15, 5, 13, 7, 1) dá 41. A resposta é Falso.
```

Tenha em mente que os autores afirmam que esta é uma habilidade emergente que surge com modelos de linguagem suficientemente grandes.

## **Prompt COT Zero-Shot**

[https://www.promptingguide.ai/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fzero-cot.79793bee.png&w=1920&q=75](https://www.promptingguide.ai/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fzero-cot.79793bee.png&w=1920&q=75)

Fonte da imagem: [Kojima et al. (2022)](https://arxiv.org/abs/2205.11916)

Uma ideia recente que surgiu mais recentemente é a ideia de [zero-shot CoT](https://arxiv.org/abs/2205.11916) (Kojima et al. 2022) que envolve essencialmente adicionar "Vamos pensar passo a passo" para a solicitação original. Vamos tentar um problema simples e ver como o modelo funciona:

*Prompt:*

```
Fui ao mercado e comprei 10 maçãs. Dei 2 maçãs ao vizinho e 2 ao reparador. Então fui comprar mais 5 maçãs e comi 1. Com quantas maçãs fiquei?
```

*Saída:*

```
11 maçãs
```

A resposta está incorreta! Agora vamos tentar com o prompt especial.

*Prompt:*

```
Fui ao mercado e comprei 10 maçãs. Dei 2 maçãs ao vizinho e 2 ao reparador. Então fui comprar mais 5 maçãs e comi 1. Com quantas maçãs fiquei?Vamos pensar passo a passo.
```

*Saída:*

```
Primeiro, você começou com 10 maçãs.Você deu 2 maçãs para o vizinho e 2 para o reparador, então você tinha 6 maçãs sobrando.Então você comprou mais 5 maçãs, então agora você tinha 11 maçãs.Finalmente, você comeu 1 maçã, então ficaria com 10 maçãs.
```

É impressionante que esse prompt simples seja eficaz nessa tarefa. Isso é particularmente útil onde você não tem muitos exemplos para usar no prompt.