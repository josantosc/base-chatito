# Chatito

O Chatito ajuda a gerar conjuntos de dados para treinamento e validação de modelos de chatbot usando um DSL simples.

Se você está construindo chatbots usando modelos comerciais, estruturas de código aberto ou escrevendo seu próprio modelo de processamento de linguagem natural, você precisa de exemplos de treinamento e teste. Chatito está aqui para ajudá-lo.

### RASA

Rasa é uma estrutura de aprendizado de máquina de código aberto para texto automatizado e conversas baseadas em voz. Entenda mensagens, mantenha conversas e conecte-se a canais de mensagens e APIs. Chatito pode ajudá-lo a construir um conjunto de dados para o componente Rasa NLU .

Um comportamento particular do adaptador Rasa é que quando uma frase de definição de slot contém apenas um alias e esse alias define o argumento 'sinônimo' com 'true', o conjunto de dados Rasa gerado mapeará o alias como um sinônimo. por exemplo:

```
%[some intent]('training': '1')
    @[some slot]

@[some slot]
    ~[some slot synonyms]

~[some slot synonyms]('synonym': 'true')
    synonym 1
    synonym 2
```
Neste exemplo, o conjunto de dados Rasa gerado conterá o `entity_synonyms` de `synonym 1` e `synonym 2` mapeado para `some slot synonyms`.

### Formato padrão

Use o formato padrão se você planeja treinar um modelo customizado ou se estiver escrevendo um adaptador customizado. Este é o formato mais flexível porque você pode anotar `Slots` e `Intents` com argumentos de entidade personalizados, e todos eles estarão presentes na saída gerada, então, por exemplo, você também pode incluir lógica de geração de diálogo / resposta com o DSL. Por exemplo:

```
%[some intent]('context': 'some annotation')
    @[some slot] ~[please?]

@[some slot]('required': 'true', 'type': 'some type')
    ~[some alias here]

```

Entidades personalizadas como 'contexto', 'obrigatório' e 'tipo' estarão disponíveis na saída para que você possa manipular esses argumentos personalizados como desejar.


## Instalação

Chatito suporta Node.js `>= v8.11`.

Instale-o com yarn ou npm:
`npm i chatito --save`

Em seguida, crie um arquivo de definição (por exemplo trainClimateBot.chatito:) com seu código.

Execute o gerador de npm:

`npx chatito trainClimateBot.chatito`
O conjunto de dados gerado deve estar disponível próximo ao seu arquivo de definição.

Aqui estão as opções completas do gerador de npm:

```
npx chatito <pathToFileOrDirectory> --format=<format> --formatOptions=<formatOptions> --outputPath=<outputPath> --trainingFileName=<trainingFileName> --testingFileName=<testingFileName> --defaultDistribution=<defaultDistribution> --autoAliases=<autoAliases>
```

- `<pathToFileOrDirectory>`caminho para um `.chatitoarquivo` ou diretório que contém arquivos chatito. Se for um diretório, irá pesquisar recursivamente todos os `*.chatitoarquivos` dentro dele e usá-los para gerar o conjunto de dados. por exemplo: `lightsChange.chatito` ou `./chatitoFilesFolder`

- `<format>` Opcional. `default, rasa, luis, flairOu snips.`

- `<formatOptions>` Opcional. Caminho para um arquivo .json que cada adaptador pode usar opcionalmente

- <outputPath>Opcional. O diretório onde salvar os conjuntos de dados gerados. Usa o diretório atual como padrão.

- <trainingFileName>Opcional. O nome do arquivo de conjunto de dados de treinamento gerado. Não se esqueça de adicionar uma extensão .json no final. Usa <format>_dataset_training.json como nome de arquivo padrão.

- `<testingFileName>` Opcional. O nome do arquivo de conjunto de dados de teste gerado. Não se esqueça de adicionar uma extensão .json no final. Usa `<format>` _dataset_testing.json como nome de arquivo padrão.

- `<defaultDistribution>` Opcional. A distribuição de frequência padrão se não for definida no nível da entidade. O padrão é `regular` e pode ser definido como `even`.

- `<autoAliases>` Opcional. O comportamento geral ao localizar um alias indefinido. Opions válidos são `allow, warn, restrict`. Padrão para 'permitir'.

