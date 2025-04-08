Tutorial de desenvolvimento do Qlik Sense Mashup 

https://community.qlik.com/t5/Qlik-Sense-Documents/Qlik-Sense-Mashup-Development-Tutorial/ta-p/1576213

Passos para obter o layout e o posicionamento dos objetos dentro de um app:

1. Obter o Layout do App
2. Primeiro, você pode obter o layout geral do aplicativo, que inclui informações sobre a estrutura e a configuração do app.

Exemplo para obter o layout do app:

javascript
Copiar
app.getAppLayout(function(layout) {
    console.log('App Layout:', layout);
    console.log('Title:', layout.qTitle);
    console.log('Last Reload Time:', layout.qLastReloadTime);
});
Isso retorna informações gerais sobre o aplicativo, como o título do app e o tempo da última recarga.

2. Obter o Layout de uma Sheet
Para obter o layout específico de uma sheet (tela dentro do aplicativo), você pode usar o método getSheet() ou getList('SheetList'). Isso trará detalhes sobre o conteúdo da sheet, incluindo a posição dos objetos.

Por exemplo:

javascript
Copiar
app.getList('SheetList', function(reply) {
    reply.qSheetList.qItems.forEach(function(sheet) {
        console.log('Sheet Title:', sheet.qData.title);
        console.log('Sheet ID:', sheet.qInfo.qId);

        // Obter layout da Sheet
        app.getSheet(sheet.qInfo.qId, function(sheetLayout) {
            console.log('Sheet Layout:', sheetLayout);
            // O layout de uma sheet contém informações sobre a disposição dos objetos dentro dela.
        });
    });
});

3. Obter Objetos Dentro de uma Sheet
Dentro de uma sheet, cada objeto (gráficos, tabelas, etc.) tem suas próprias configurações de layout, como posição, tamanho, etc. Você pode usar o método getObjects() para acessar os objetos de uma sheet e obter detalhes sobre eles, incluindo o posicionamento.

Exemplo de como pegar o layout de objetos dentro de uma sheet:

javascript
Copiar
app.getList('SheetList', function(reply) {
    reply.qSheetList.qItems.forEach(function(sheet) {
        // Obter objetos dentro de uma sheet
        app.getSheet(sheet.qInfo.qId, function(sheetData) {
            sheetData.getObjects(function(objects) {
                objects.forEach(function(obj) {
                    console.log('Object ID:', obj.qInfo.qId);
                    console.log('Object Title:', obj.qData.title);
                    console.log('Object Position:', obj.qPosition); // Posição do objeto
                    console.log('Object Size:', obj.qSize); // Tamanho do objeto
                });
            });
        });
    });
});

4. Detalhes sobre Posição e Tamanho do Objeto
Quando você usa o método getObjects(), cada objeto retornado possui propriedades como:

qPosition: Informações sobre a posição do objeto na tela (geralmente inclui valores como x, y, que representam a posição do objeto).

qSize: Tamanho do objeto, incluindo largura e altura.

Exemplo de como obter posição e tamanho:
javascript
Copiar
objects.forEach(function(obj) {
    console.log('Object ID:', obj.qInfo.qId);
    console.log('Object Position:', obj.qPosition); // Ex: {x: 100, y: 200}
    console.log('Object Size:', obj.qSize); // Ex: {width: 300, height: 200}
});

5. Exemplo Completo: Obter Layout e Posição dos Objetos em uma Sheet
Aqui está um exemplo completo que pega o layout do app, as sheets e os objetos dentro dessas sheets, incluindo suas posições e tamanhos:

javascript
Copiar
require(["js/qlik"], function(qlik) {
    var config = {
        host: window.location.hostname,
        port: window.location.port,
        isSecure: window.location.protocol === "https:",
        prefix: window.location.pathname.substr(0, window.location.pathname.toLowerCase().lastIndexOf("/extensions") + 1)
    };

    // Conectar ao Qlik
    var app = qlik.openApp('c3707626-af3d-4592-8f8d-8ca8d289fd99', config); // Use o ID do app

    // Obter Layout do App
    app.getAppLayout(function(layout) {
        console.log('App Layout:', layout);
    });

    // Obter todas as Sheets no App
    app.getList('SheetList', function(reply) {
        reply.qSheetList.qItems.forEach(function(sheet) {
            console.log('Sheet Title:', sheet.qData.title);
            console.log('Sheet ID:', sheet.qInfo.qId);

            // Obter Objetos dentro da Sheet
            app.getSheet(sheet.qInfo.qId, function(sheetData) {
                sheetData.getObjects(function(objects) {
                    objects.forEach(function(obj) {
                        console.log('Object ID:', obj.qInfo.qId);
                        console.log('Object Title:', obj.qData.title);
                        console.log('Object Position:', obj.qPosition); // Posição do objeto
                        console.log('Object Size:', obj.qSize); // Tamanho do objeto
                    });
                });
            });
        });
    });
});

Explicação:

getAppLayout(): Obtém o layout geral do app, que pode incluir informações como título e última recarga.

getList('SheetList'): Obtém a lista de sheets dentro do app, incluindo o ID de cada sheet.

getSheet(): Acessa o layout de uma sheet específica.

getObjects(): Obtém os objetos dentro de uma sheet, incluindo as propriedades de posição (qPosition) e tamanho (qSize).

6. Verifique no Console do Navegador
   
Após executar o código acima, você pode visualizar as informações de layout e posição no console do navegador (pressione F12 para abrir as ferramentas de desenvolvedor e vá para a aba Console).

Isso permitirá que você veja os IDs, títulos, posições e tamanhos dos objetos dentro do Qlik Sense.

Conclusão:
Para pegar o layout e o posicionamento dos objetos dentro de um aplicativo no Qlik Sense, você pode usar os métodos da API como getAppLayout(), getList('SheetList'), getSheet() e getObjects(). Esses métodos permitem obter as propriedades detalhadas de cada objeto, incluindo a posição e o tamanho dentro das sheets.

Se precisar de mais ajuda para ajustar o código ou encontrar mais informações sobre a API do Qlik, fique à vontade para perguntar!
