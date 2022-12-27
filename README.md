# Script Shortener Links on Google Sheets - API Bitly

> The objective of this step by step is to use Google Sheets to shorten mass links through the API provided by Bitly, only needing to provide all the URLs that you want to shorten and using the Google Sheets "drag" functionality.

> O objetivo deste passo a passo é utilizar o Google Sheets para encurtar links em massa através da API disponibilizada pelo Bitly, necessitando apenas prover todas as URLs que deseja-se encurtar e utilizando da funcionalidade do Google Sheets de "arrasto".

## Step 01:
<details>
<summary>:us: English</summary>
Create a sheet (or use a existent one) on Google Sheets;
</details>
<details>
<summary>:brazil: Português</summary>
Criar uma planilha (ou planilha já criada) no Google Sheets;
</details>
<details>
<summary>:camera_flash: Image | Imagem</summary>

##### Nova Planilha | New Sheet
![new-sheet](/images/01-empty-sheet.png)
</details>

## Step 02:
<details>
<summary>:us: English</summary>
Search on nav bar "Extensions" > "Apps Script" and create a new project (e.g.: bitlyGenerator.gs);
</details>
<details>
<summary>:brazil: Português</summary>
Na planilha busque no menu superior Extensões > Apps Script e crie um novo projeto (ex.: bitlyGenerator.gs);
</details>
<details>
<summary>:camera_flash: Image | Imagem</summary>

##### Acessando Apps Script | Opening Apps Script
![extension-apps-script](/images/02-extensions-app-script.png)

##### Organizando o Ambiente de Trabalho | Organizing Workspace
![new-app-script](/images/03-new-app-script.png)
</details>

## Step 03:
<details>
<summary>:us: English</summary>
Copy the code below and save it for use;
</details>
<details>
<summary>:brazil: Português</summary>
Copiar o código disponibilizado no arquivo e salvá-lo para utilização;
</details>
<details>
<summary>:camera_flash: Image | Imagem</summary>

##### Criando a Função | Creating a Function
![app-script-function](/images/04-app-script-function.png)
</details>

## Step 04:
<details>
<summary>:us: English</summary>
On the bitly website, with an active account, open the development tab (Settings > Developer Settings > API > Enter Password > Generate Token) and generate the API authentication token;
</details>
<details>
<summary>:brazil: Português</summary>
No site bitly, com a conta ativa, abrir a aba development (Settings > Developer Settings > API > Enter Password > Generate Token) e gerar o token de autenticação da API;
</details>
<details>
<summary>:camera_flash: Image | Imagem</summary>

##### Acessando o Bitly | Accessing Bitly
![bitly-account](/images/05-bitly-account.png)
	
##### Token API Bitly - Parte 1 | Token API Bitly - Part 1
![bitly-token-api](/images/06-bitly-token-api.png)
	
##### Token API Bitly - Parte 2 | Token API Bitly - Part 2
![bitly-token-password](/images/07-bitly-token-password.png)
	
##### Token API Bitly - Parte 3 | Token API Bitly - Part 3
![bitly-token-generate](/images/08-bitly-token-generate.png)

##### Token API Bitly - Parte 4 | Token API Bitly - Part 4
![bitly-token](/images/09-bitly-token.png)
</details>

## Step 05:
<details>
<summary>:us: English</summary>
In the worksheet, use the function named in Apps Script in the cell through <b>=getBitly(arg1;arg2)</b>, where the first argument refers to the URL to be shortened, and the second argument refers to the token accessed in Step 04;
</details>
<details>
<summary>:brazil: Português</summary>
Na planilha utilizar a função nomeada no Apps Script na célula por meio de <b>=getBitly(arg1;arg2)</b>, onde o primeiro argumento se refere a URL que se deseja encurtar, e o segundo argumento se refere ao token acessado no Passo 04;
</details>
<details>
<summary>:camera_flash: Image | Imagem</summary>

##### Aplicando a Função na Planilha | Applying the Function on the Sheet
![bitly-token](/images/10-sheets-function.png)
	
##### Utilizando o "arrastar" | Using "drag" function
![bitly-token](/images/11-sheets-massively-shortened.png)
	
##### Resultado dos Sites encurtados | Result of shortened Sites
![bitly-token](/images/12-sheets-shortlinks-results.png)
</details>
 
## :brazil: Código | :us: Script:

```
function  getBitly(URL, Token) {
	let  bitlyurl = "https://api-ssl.bitly.com/v4/shorten";
	let  bitlyarr = [];
	if (Array.isArray(URL)){
		for (let  i = 0; i < URL.length; i++) {
			if (URL[i] == "") {
				bitlyarr.push("Empty Cell");
				continue;
			}
			if(URL[i].toString().match(/^http[\ss]\:\/\//gi)){
				let  l_url = URL[i].toString();
			} else {
				l_url = "https://" + URL[i];
			}
			let  long_url = {
				long_url: l_url
			};
			let  options = {
				headers: {Authorization: 'Bearer '+ Token},
				muteHttpExceptions: true,
				contentType: 'application/json',
				method: 'POST',
				payload : JSON.stringify(long_url)
			};
			let  response = UrlFetchApp.fetch(bitlyurl, options);
			if (response.getResponseCode() == 200 || response.getResponseCode() == 201){
				let  response_Json = JSON.parse(response.getContentText());
				let  bitlyShort = response_Json.link;
				bitlyarr.push(bitlyShort);
			} else {
				bitlyarr.push(response.getContentText());
			}
		}
	} else {
		if (URL == "") {
			return"Empty Cell"
		}
		let  long_url = {
			long_url: URL
		};
		let  options = {
			headers: {Authorization: 'Bearer '+ Token},
			muteHttpExceptions: true,
			contentType: 'application/json',
			method: 'POST',
			payload : JSON.stringify(long_url)
		};
		let  response = UrlFetchApp.fetch(bitlyurl, options);
		if (response.getResponseCode() == 200 || response.getResponseCode() == 201){
			let  response_Json = JSON.parse(response.getContentText());
			let  bitlyShort = response_Json.link;
			bitlyarr.push(bitlyShort);
		} else {
			bitlyarr.push(response.getContentText());
		}
	}
	return  bitlyarr;
}
```
