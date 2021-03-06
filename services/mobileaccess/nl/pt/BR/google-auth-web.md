---

copyright:
  year: 2016

---

# Ativando a autenticação do Google para aplicativos da web
{: #google-auth-web}

Última atualização: 18 de julho de 2016
{: .last-updated}

Use o Google Sign-In para autenticar usuários em seu aplicativo da web. Inclua a funcionalidade de segurança do {{site.data.keyword.amashort}}. 


## Antes de começar
{: #before-you-begin}

Você deve ter:
* Um app da web.
* Uma instância de um aplicativo {{site.data.keyword.Bluemix_notm}} que seja protegida pelo serviço {{site.data.keyword.amashort}}. Para obter mais informações sobre como criar um aplicativo backend do {{site.data.keyword.Bluemix_notm}}, consulte [Introdução](index.html).
* O URI para o redirecionamento final (após o processo de autorização ser concluído).



## Configurando um aplicativo Google para seu website
Para iniciar o uso do Google como um provedor de identidade, crie um projeto no [Console do desenvolvedor do Google](https://console.developers.google.com). Parte
da criação de um projeto é obter um **Identificador de cliente do Google** e um **Segredo**. O Identificador
de cliente do Google e o Segredo são os identificadores exclusivos para seu aplicativo usados pela autenticação do Google e são necessários para
configurar o painel {{site.data.keyword.amashort}}.


1. Abra seu aplicativo Google no Google Developer Console. 
3. Inclua a API do Google+. 
3. Crie credenciais usando o OAuth. Selecione o aplicativo da web no tipo de aplicativo. Insira o URI de redirecionamento
{{site.data.keyword.amashort}} na caixa de URIs de redirecionamento autorizado. O URI de autorização de redirecionamento
{{site.data.keyword.amashort}} pode ser obtido a partir da tela de configuração do Google do painel {{site.data.keyword.amashort}}
(consulte as etapas abaixo). 
6. Salve as mudanças. Anote o identificador de cliente do Google e o Segredo do aplicativo.


## Configurando o {{site.data.keyword.amashort}} para autenticação do Google
Depois de ter um ID de aplicativo e um segredo do Google, é possível ativar a autenticação do Google no painel do {{site.data.keyword.amashort}}.

1. Abra seu app no painel do {{site.data.keyword.Bluemix_notm}}.
1. Clique no ladrilho {{site.data.keyword.amashort}}. O painel do {{site.data.keyword.amashort}} é carregado.
1. Clique no botão no painel do Google.
1. Na seção **Configurar para web**:   
    * Observe o valor na caixa de texto **URI de redirecionamento do Mobile Client Access para o Google Developer Console**. Esse é o valor necessário para incluir na caixa **URIs de redirecionamento autorizado** em **Restrições no Identificador de cliente para aplicativo da web** do **Portal de desenvolvedores do Google** na etapa 3.
    * Insira o **Identificador de cliente do Google** e o **Segredo do cliente**.
    * Insira o URI de redirecionamento nos **URIs de redirecionamento de aplicativo da web**. Esse valor é para que o URI de
redirecionamento seja acessado após o processo de autorização ser concluído e é determinado pelo desenvolvedor.
1. Clique em **Salvar**.


## Implementando o fluxo de autorização {{site.data.keyword.amashort}} usando o Google como provedor de identidade

A variável de ambiente `VCAP_SERVICES` é criada automaticamente para cada instância de serviço do
{{site.data.keyword.amashort}} e contém propriedades necessárias para o processo de autorização. Ela consiste em um objeto JSON e pode ser
visualizada clicando em **Variáveis de ambiente** no navegador esquerdo do seu aplicativo.

Para iniciar o processo de autorização:

1. Recupere o terminal de autorização (`authorizationEndpoint`) e o clientId (`clientId`) das credenciais
de serviço armazenadas na variável de ambiente `VCAP_SERVICES`. 

    **Nota:** se você criou o serviço {{site.data.keyword.amashort}} antes de o suporte da web ter sido incluído, talvez você não tenha terminais de autorização nas credenciais de serviço. Nesse caso, use os terminais de autorização a seguir, dependendo da região do Bluemix:


 	Sul dos EUA: 
 	```
 	https://mobileclientaccess.ng.bluemix.net/oauth/v2/authorization
 	```
 	Londres: 
 	```
 	https://mobileclientaccess.eu-gb.bluemix.net/oauth/v2/authorization
  	```
  	Sydney: 
  	```
  	https://mobileclientaccess.au-syd.bluemix.net/oauth/v2/authorization
  	```
2. Construa o URI do servidor de autorizações usando `response_type("code")`, `client_id` e `redirect_uri` como parâmetros de consulta.
3. Redirecione a partir do seu aplicativo da web para o URI gerado.
  
O exemplo a seguir recupera os parâmetros da variável `VCAP_SERVICES`, construindo a URL e enviando a solicitação de redirecionamento.
  
```Java
 var cfEnv = require("cfenv"); 
 app.get("/protected", checkAuthentication, function(req, res, next){ 
 	res.send("Hello from protected endpoint"); 
 }); 

 app.get("/protected", checkAuthentication, function(req, res, next){ 
 	res.send("Hello from protected endpoint"); 
 	function checkAuthentication(req, res, next){ 

	// Check if user is authenticated 
 	if (req.session.userIdentity){ 
		next() 
	} else { 
		// If not - redirect to authorization server 
		var mcaCredentials = cfEnv.getAppEnv().services.AdvancedMobileAccess[0].credentials; 
		var authorizationEndpoint = mcaCredentials.authorizationEndpoint; 
		var clientId = mcaCredentials.clientId; 
		var redirectUri = "http://some-server/oauth/callback"; // Your web application redirect URI 
		var redirectUrl = authorizationEndpoint + "?response_type=code";
		redirectUrl += "&client_id=" + clientId; 
		redirectUrl += "&redirect_uri=" + redirectUri; 
		res.redirect(redirectUrl); 
	} 
} 
```

Observe que o parâmetro `redirect_uri` representa seu URI de aplicativo da web e deve ser igual àquele definido no painel
{{site.data.keyword.amashort}}.
Após redirecionar para o terminal de autorização, o usuário obterá um formulário de login do Google. Depois que o usuário concede permissões para efetuar login usando a identidade do Google, o serviço {{site.data.keyword.amashort}} chama o URI de redirecionamento de aplicativo da web, fornecendo o código de concessão como um parâmetro de consulta.

## Obtendo os tokens
A próxima etapa é obter token de acesso e tokens de identidade usando o código de concessão recebido anteriormente. 

1. Recupere os tokens `tokenEndpoint`, `clientId` e `secret` das credenciais de serviço armazenadas na variável de ambiente `VCAP_SERVICES`. 
  
    **Nota:** se você criou o serviço {{site.data.keyword.amashort}} antes de o suporte da web ter sido incluído, talvez você não tenha terminais de autorização nas credenciais de serviço. Nesse caso, use os terminais de autorização a seguir, dependendo da região do Bluemix:
 
 	Sul dos EUA: 
 	```
 	https:// mobileclientaccess.ng.bluemix.net/oauth/v2/token 
  	```
 	Londres: 
  	```
  	https:// mobileclientaccess.eu-gb.bluemix.net/oauth/v2/token  
   	```
   	Sydney: 
  	```
   	https:// mobileclientaccess.au-syd.bluemix.net/oauth/v2/token 
  	```

2. Envie uma solicitação de post ao URI do servidor de token com grant_type ("authorization_code"), client_id, redirect_uri e code como parâmetros de formulário. Envie
`clientId` e `clientSecret` como Credenciais de autenticação HTTP básicas.
 
O código a seguir recupera os valores necessários e os envia com uma solicitação de post.
    
```Java    
  var cfEnv = require("cfenv");
  var base64url = require("base64url ");
  var request = require('request');

   app.get("/oauth/callback", function(req, res, next){ 
	var mcaCredentials = cfEnv.getAppEnv().services.AdvancedMobileAccess[0].credentials; 
	var tokenEndpoint = mcaCredentials.tokenEndpoint; 
	var formData = { 
		grant_type: "authorization_code", 
		client_id: mcaCredentials.clientId, 
		redirect_uri: "http://some-server/oauth/callback",// Your web application redirect uri 
		code: req.query.code 
	} 

	request.post({ 
		url: tokenEndpoint, 
		formData: formData 
		}, function (err, response, body){ 
			var parsedBody = JSON.parse(body); 
			req.session.accessToken = parsedBody.access_token; 
			req.session.idToken = parsedBody.id_token; 
			var idTokenComponents = parsedBody.id_token.split("."); // [header, payload, signature] 
			var decodedIdentity= base64url(idTokenComponents[1]);
			req.session.userIdentity = JSON.parse(decodedIdentity)["imf.user"]; 
			res.redirect("/"); 
			}
	).auth(mcaCredentials.clientId, mcaCredentials.secret); 
  }
); 
```

O parâmetro `redirect_uri` é o URI para redirecionar, após a autenticação bem-sucedida ou com falha com o Google+ e deve
corresponder ao `redirect_uri` a partir da etapa 1.  
   
Certifique-se de enviar esta solicitação POST no período de 10 minutos, após o qual, o código de concessão expirará. Depois de 10 minutos, será necessário um novo código.

O corpo de resposta POST contém o `access_token` e o `id_token` codificados em base64.

Depois de receber o acesso e os tokens de identidade, será possível sinalizar a sessão da web como autenticada e, opcionalmente, persistir esses tokens.  


##Usando o acesso e o token de identidade obtidos 

O token de identidade contém informações sobre a identidade do usuário. Para autenticação do Google, o token contém todas as informações que o usuário concordou em compartilhar, como nome completo, URL da foto de perfil, etc.  

O token de acesso permite a comunicação com os recursos protegidos pelos filtros de autorização do {{site.data.keyword.amashort}}. Leia sobre [Protegendo recursos](protecting-resources.html).

Para fazer solicitações para recursos protegidos, inclua um cabeçalho de autorização nas solicitações com a estrutura a seguir: 

`Authorization=Bearer <accessToken> <idToken>`

####Dicas:
{: tips} 

* O `accessToken` e o `idToken` devem ser separados por um espaço em branco.

* O `idToken` é opcional. Se você não fornecer o token de identidade, o recurso protegido poderá ser acessado, mas não receberá nenhuma informação sobre o usuário autorizado. 



