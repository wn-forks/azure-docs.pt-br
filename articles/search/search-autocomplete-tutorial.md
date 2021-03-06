---
title: Adicionar preenchimento automático e sugestões em uma caixa de pesquisa
titleSuffix: Azure Cognitive Search
description: Habilite as ações de consulta de pesquisa conforme o tipo no Azure Pesquisa Cognitiva criando sugestores e solicitações formulars que completam a caixa de pesquisa com termos ou frases concluídas. Você também pode retornar correspondências sugeridas.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/08/2020
ms.custom: devx-track-javascript, devx-track-csharp
ms.openlocfilehash: 8be53838f6262eaafc643bc78fd08b6f02d9bac6
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89660257"
---
# <a name="add-autocomplete-and-suggestions-to-client-apps"></a>Adicionar preenchimento automático e sugestões aos aplicativos cliente

O Search-as-Type é uma técnica comum para melhorar a produtividade das consultas iniciadas pelo usuário. No Azure Pesquisa Cognitiva, essa experiência tem suporte por meio de *preenchimento automático*, que termina um termo ou frase com base na entrada parcial (concluindo "micro" com "Microsoft"). Outro formulário é *sugestões*: uma lista curta de documentos correspondentes (retornando títulos de livros com uma ID para que você possa vincular a uma página de detalhes). O preenchimento automático e as sugestões são predicadas em uma correspondência no índice. O serviço não oferecerá consultas que retornam zero resultados.

Para implementar essas experiências no Azure Pesquisa Cognitiva, será necessário:

+ Um *Sugestor* no back-end.
+ Uma *consulta* que especifica a API de [sugestões](/rest/api/searchservice/suggestions) ou de [preenchimento automático](/rest/api/searchservice/autocomplete) na solicitação.
+ Um *controle de interface do usuário* para manipular interações de pesquisa conforme o tipo no aplicativo cliente. É recomendável usar uma biblioteca JavaScript existente para essa finalidade.

No Azure Pesquisa Cognitiva, as consultas autocompletadas e os resultados sugeridos são recuperados do índice de pesquisa, dos campos selecionados que você registrou com um Sugestor. Um Sugestor faz parte do índice e especifica quais campos fornecerão conteúdo que conclui uma consulta, sugere um resultado ou ambos. Quando o índice é criado e carregado, uma estrutura de dados de sugestão é criada internamente para armazenar prefixos usados para correspondência em consultas parciais. Para sugestões, escolher campos adequados que sejam exclusivos, ou pelo menos não repetitivo, é essencial para a experiência. Para obter mais informações, consulte [criar um Sugestor](index-add-suggesters.md).

O restante deste artigo tem como foco as consultas e o código do cliente. Ele usa JavaScript e C# para ilustrar os principais pontos. Exemplos de API REST são usados para representar de forma concisa cada operação. Para obter links para exemplos de código de ponta a ponta, consulte [próximas etapas](#next-steps).

## <a name="set-up-a-request"></a>Configurar uma solicitação

Os elementos de uma solicitação incluem uma das APIs de pesquisa conforme o tipo, uma consulta parcial e um Sugestor. O script a seguir ilustra os componentes de uma solicitação, usando a API REST de preenchimento automático como um exemplo.

```http
POST /indexes/myxboxgames/docs/autocomplete?search&api-version=2020-06-30
{
  "search": "minecraf",
  "suggesterName": "sg"
}
```

O **suggesterName** fornece os campos que reconhecem o sugestores usados para concluir os termos ou sugestões. Para sugestões em particular, a lista de campos deve ser composta por aquelas que oferecem opções claras entre os resultados correspondentes. Em um site que vende jogos de computador, o campo pode ser o título do jogo.

O parâmetro de **pesquisa** fornece a consulta parcial, em que os caracteres são alimentados para a solicitação de consulta por meio do controle de preenchimento automático do jQuery. No exemplo acima, "minecraf" é uma ilustração estática do que o controle pode ter passado.

As APIs não impõem requisitos de comprimento mínimo na consulta parcial; Ele pode ser tão pequeno quanto um caractere. No entanto, a autocompletação do jQuery fornece um comprimento mínimo. Um mínimo de dois ou três caracteres é típico.

As correspondências estão no início de um termo em qualquer lugar na cadeia de caracteres de entrada. Dada "The Quick Brown raposa", o preenchimento automático e as sugestões serão correspondentes em versões parciais de "The", "Quick", "Brown" ou "Fox", mas não em termos de infixos parciais como "rown" ou "Ox". Além disso, cada correspondência define o escopo para expansões downstream. Uma consulta parcial de "Quick br" corresponderá a "Quick Brown" ou "pão rápido", mas nem "Brown" ou "pão" por si só seriam correspondentes, a menos que "rápido" os preceda.

### <a name="apis-for-search-as-you-type"></a>APIs para pesquisa conforme o tipo

Siga estes links para as páginas de referência REST e SDK do .NET:

+ [API REST de sugestões](/rest/api/searchservice/suggestions) 
+ [API REST de preenchimento automático](/rest/api/searchservice/autocomplete) 
+ [Método SuggestWithHttpMessagesAsync](/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet)
+ [Método AutocompleteWithHttpMessagesAsync](/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet)

## <a name="structure-a-response"></a>Estruturar uma resposta

As respostas para preenchimento automático e sugestões são as que você pode esperar para o padrão: o [preenchimento automático](/rest/api/searchservice/autocomplete#response) retorna uma lista de termos, as [sugestões](/rest/api/searchservice/suggestions#response) retornam termos mais uma ID de documento para que você possa buscar o documento (use a API de [documento de pesquisa](/rest/api/searchservice/lookup-document) para buscar o documento específico para uma página de detalhes).

As respostas são moldadas pelos parâmetros na solicitação. Para preenchimento automático, defina [**autocompleteMode**](/rest/api/searchservice/autocomplete#autocomplete-modes) para determinar se a conclusão do texto ocorre em um ou dois termos. Para sugestões, o campo escolhido determina o conteúdo da resposta.

Para sugestões, você deve refinar ainda mais a resposta para evitar duplicatas ou o que parece ser de resultados não relacionados. Para controlar os resultados, inclua mais parâmetros na solicitação. Os parâmetros a seguir se aplicam a AutoCompletar e sugestões, mas talvez sejam mais necessários para sugestões, especialmente quando um Sugestor inclui vários campos.

| Parâmetro | Uso |
|-----------|-------|
| **$select** | Se você tiver vários **sourceFields** em um Sugestor, use **$Select** para escolher qual campo contribui com valores ( `$select=GameTitle` ). |
| **searchFields** | Restringir a consulta a campos específicos. |
| **$filter** | Aplicar critérios de correspondência no conjunto de resultados ( `$filter=Category eq 'ActionAdventure'` ). |
| **$top** | Limitar os resultados a um número específico ( `$top=5` ).|

## <a name="add-user-interaction-code"></a>Adicionar código de interação do usuário

O preenchimento automático de um termo de consulta ou a remoção de uma lista de links correspondentes requer o código de interação do usuário, normalmente o JavaScript, que pode consumir solicitações de fontes externas, como preenchimento automático ou consultas de sugestão em um Azure Search índice cognitiva.

Embora você possa escrever esse código nativamente, é muito mais fácil usar funções da biblioteca JavaScript existente. Este artigo demonstra dois, um para sugestões e outro para preenchimento automático. 

+ O [widget preenchimento automático (IU do jQuery)](https://jqueryui.com/autocomplete/) é usado no exemplo de sugestão. Você pode criar uma caixa de pesquisa e, em seguida, referenciá-la em uma função JavaScript que usa o widget preenchimento automático. As propriedades no widget definem a origem (uma função de AutoCompletar ou de sugestões), o comprimento mínimo dos caracteres de entrada antes que a ação seja executada e o posicionamento.

+ O [plug-in de preenchimento automático do XDSoft](https://xdsoft.net/jqplugins/autocomplete/) é usado no exemplo de preenchimento automático.

Usamos essas bibliotecas para criar a caixa de pesquisa com suporte para sugestões e preenchimento automático. As entradas coletadas na caixa de pesquisa são emparelhadas com sugestões e ações de preenchimento automático.

## <a name="suggestions"></a>Sugestões

Esta seção orienta você pela implementação de resultados sugeridos, começando com a definição da caixa de pesquisa. Ele também mostra como e script que invoca a primeira biblioteca de preenchimento automático de JavaScript referenciada neste artigo.

### <a name="create-a-search-box"></a>Criar uma caixa de pesquisa

Supondo que a [biblioteca de preenchimento automático da interface do usuário do jQuery](https://jqueryui.com/autocomplete/) e um projeto do MVC em C#, você pode definir a caixa de pesquisa usando JavaScript no arquivo **index. cshtml** . A biblioteca adiciona a interação de pesquisa conforme o tipo à caixa de pesquisa, fazendo chamadas assíncronas para o controlador MVC para recuperar sugestões.

Em **index. cshtml** sob a pasta \Views\Home, uma linha para criar uma caixa de pesquisa pode ser a seguinte:

```html
<input class="searchBox" type="text" id="searchbox1" placeholder="search">
```

Esse exemplo é uma caixa de texto de entrada simples com uma classe para definir o estilo, uma ID para referência pelo JavaScript e o texto de espaço reservado.  

Dentro do mesmo arquivo, incorpore o JavaScript que faz referência à caixa de pesquisa. A função a seguir chama a API de sugestão, que solicita documentos correspondentes sugeridos com base em entradas de termo parcial:

```javascript
$(function () {
    $("#searchbox1").autocomplete({
        source: "/home/suggest?highlights=false&fuzzy=false&",
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

O `source` informa à função preenchimento automático da interface do usuário do jQuery onde obter a lista de itens a serem mostrados na caixa de pesquisa. Como esse projeto é um projeto do MVC, ele chama a função de **sugestão** em **HomeController.cs** que contém a lógica para retornar sugestões de consulta. Essa função também passa alguns parâmetros para controlar destaques, correspondências difusas e termos. A API JavaScript de preenchimento automático adiciona o parâmetro de termo.

O `minLength: 3` garante que as recomendações serão mostradas apenas quando houver pelo menos três caracteres na caixa de pesquisa.

### <a name="enable-fuzzy-matching"></a>Habilitar correspondência difusa

A pesquisa difusa permite que você obtenha resultados com base em correspondências próximas, mesmo se o usuário errou a escrita de uma palavra na caixa de pesquisa. A distância de edição é 1, o que significa que pode haver uma discrepância máxima de um caractere entre a entrada do usuário e uma correspondência. 

```javascript
source: "/home/suggest?highlights=false&fuzzy=true&",
```

### <a name="enable-highlighting"></a>Habilitar realce

O realce aplica o estilo da fonte aos caracteres no resultado que correspondem à entrada. Por exemplo, se a entrada parcial for "micro", o resultado apareceria como **micro**Soft, **micro**Scope e assim por diante. O realce é baseado nos parâmetros HighlightPreTag e HighlightPostTag, definidos embutidos com a função Suggestion.

```javascript
source: "/home/suggest?highlights=true&fuzzy=true&",
```

### <a name="suggest-function"></a>Função de sugestão

Se você estiver usando C# e um aplicativo MVC, o arquivo **HomeController.cs** no diretório de controladores será onde você poderá criar uma classe para os resultados sugeridos. No .NET, uma função de sugestão é baseada no [método DocumentsOperationsExtensions. sugira](/dotnet/api/microsoft.azure.search.documentsoperationsextensions.suggest?view=azure-dotnet). Para obter mais informações sobre o SDK do .NET, consulte [como usar o Azure pesquisa cognitiva de um aplicativo .net](./search-howto-dotnet-sdk.md).

O `InitSearch` método cria um cliente de índice http autenticado para o serviço de pesquisa cognitiva do Azure. As propriedades da classe [sugiraparameters](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggestparameters) determinam quais campos são pesquisados e retornados nos resultados, o número de correspondências e se a correspondência difusa é usada. 

Para preenchimento automático, a correspondência difusa é limitada a uma distância de edição (um caractere omitido ou de local incorreto). Observe que a correspondência difusa em consultas de preenchimento automático pode, às vezes, produzir resultados inesperados dependendo do tamanho do índice e de como ele é fragmentado. Para obter mais informações, consulte [conceitos de particionamento e fragmentação](search-capacity-planning.md#concepts-search-units-replicas-partitions-shards).

```csharp
public ActionResult Suggest(bool highlights, bool fuzzy, string term)
{
    InitSearch();

    // Call suggest API and return results
    SuggestParameters sp = new SuggestParameters()
    {
        Select = HotelName,
        SearchFields = HotelName,
        UseFuzzyMatching = fuzzy,
        Top = 5
    };

    if (highlights)
    {
        sp.HighlightPreTag = "<b>";
        sp.HighlightPostTag = "</b>";
    }

    DocumentSuggestResult resp = _indexClient.Documents.Suggest(term, "sg", sp);

    // Convert the suggest query results to a list that can be displayed in the client.
    List<string> suggestions = resp.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = suggestions
    };
}
```

A função Suggest utiliza dois parâmetros que determinam se os destaques de ocorrências são retornados ou se correspondência difusa é usada em conjunto com o termo de pesquisa de entrada. O método cria um [objeto sugiraparameters](/dotnet/api/microsoft.azure.search.models.suggestparameters?view=azure-dotnet), que é passado para a API de sugestão. O resultado, em seguida, é convertido em JSON para que ele possa ser exibido no cliente.

## <a name="autocomplete"></a>Preenchimento automático

Até agora, o código UX de pesquisa foi centralizado em sugestões. O próximo bloco de código mostra o preenchimento automático, usando a função de preenchimento automático da interface do usuário do XDSoft jQuery, passando uma solicitação para preenchimento automático de Pesquisa Cognitiva do Azure. Assim como nas sugestões, em um aplicativo C#, o código que dá suporte à interação do usuário vai em **index. cshtml**.

```javascript
$(function () {
    // using modified jQuery Autocomplete plugin v1.2.6 https://xdsoft.net/jqplugins/autocomplete/
    // $.autocomplete -> $.autocompleteInline
    $("#searchbox1").autocompleteInline({
        appendMethod: "replace",
        source: [
            function (text, add) {
                if (!text) {
                    return;
                }

                $.getJSON("/home/autocomplete?term=" + text, function (data) {
                    if (data && data.length > 0) {
                        currentSuggestion2 = data[0];
                        add(data);
                    }
                });
            }
        ]
    });

    // complete on TAB and clear on ESC
    $("#searchbox1").keydown(function (evt) {
        if (evt.keyCode === 9 /* TAB */ && currentSuggestion2) {
            $("#searchbox1").val(currentSuggestion2);
            return false;
        } else if (evt.keyCode === 27 /* ESC */) {
            currentSuggestion2 = "";
            $("#searchbox1").val("");
        }
    });
});
```

### <a name="autocomplete-function"></a>Função de preenchimento automático

O preenchimento automático é baseado no [método DocumentsOperationsExtensions. AutoComplete](/dotnet/api/microsoft.azure.search.documentsoperationsextensions.autocomplete?view=azure-dotnet). Assim como acontece com as sugestões, esse bloco de código ficaria no arquivo **HomeController.cs** .

```csharp
public ActionResult AutoComplete(string term)
{
    InitSearch();
    //Call autocomplete API and return results
    AutocompleteParameters ap = new AutocompleteParameters()
    {
        AutocompleteMode = AutocompleteMode.OneTermWithContext,
        UseFuzzyMatching = false,
        Top = 5
    };
    AutocompleteResult autocompleteResult = _indexClient.Documents.Autocomplete(term, "sg", ap);

    // Convert the Suggest results to a list that can be displayed in the client.
    List<string> autocomplete = autocompleteResult.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = autocomplete
    };
}
```

A função Autocomplete usa a entrada do termo de pesquisa. O método cria um [objeto AutoCompleteParameters](/rest/api/searchservice/autocomplete). O resultado, em seguida, é convertido em JSON para que ele possa ser exibido no cliente.

## <a name="next-steps"></a>Próximas etapas

Siga estes links para obter instruções de ponta a ponta ou código que demonstram experiências de pesquisa conforme o tipo. Ambos os exemplos de código incluem implementações híbridas de sugestões e preenchimento automático em conjunto.

+ [Tutorial: criar seu primeiro aplicativo em C# (lição 3)](tutorial-csharp-type-ahead-and-suggestions.md)
+ [Exemplo de código C#: Azure-Search-dotnet-Samples/Create-First-app/3-Add-typeahead/](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v10/3-add-typeahead)
+ [C# e JavaScript com a amostra de código lado a lado do REST](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete)