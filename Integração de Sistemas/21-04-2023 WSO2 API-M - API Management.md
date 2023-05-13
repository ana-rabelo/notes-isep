
> Faremos o passo a passo de como criar, publicar, e invocar uma API usando o <span style="color:#FF7043"><b>WSO2 API Manager.</b></span>

No cenário de exemplo, implementaremos um ciclo de vida completo de uma API utilizando o WSO2 API Manager:

![[Pasted image 20230421213409.png]]

1. Criando e publicando uma API pelo `Publisher Portal do WSO2 API-M`.
2. Deploy a API no `Gateway environment`.
3. Publicar a API no `Developer Portal`.
4. Inscrevendo-se na API via `Developer Portal` do WSO2 API-M e gerando chaves.
5. Invocando a API com as `chaves` geradas.

### <span style="color:#ff8f43"><b>Antes de começar...</b></span>

1. [Download version 4.2.0 of WSO2 API-M](https://wso2.com/api-management/).
2. Inicie o WSO2 API-M navegando até o diretório `<API-M_HOME>/bin` e executando o seguinte comando na linha de comando:
	`api-manager.bat --run`

### <span style="color:#FF7043"><b>Passo 1 - Criando e publicando uma API</b></span>

1. Acesse o `Publisher Portal` <https://localhost:9443/publisher>
2. Entre com as credenciais `admin/admin`
3. Crie um serviço mock REST
- Vamos utilizar um serviço mock REST para criar uma API do zero

>[!todo]-	
>- [x] Pesquisar sobre mock

- Um serviço mock com uma resposta JSON ```{"hello" : "world"}``` é fornecida por padrão quando usando a URL <http://run.mocky.io/v2/5185415ba171ea3a00704eed>, que aparece no serviço mock <https://designer.mocky.io/>. Nesse guia vamos usar o protocolo HTTP ao invés do HTTPS.	

>[!tip] 
>Opcionalmente, para testar o serviço, copie a URL do mock e navegue pelo seu navegador.	

4. Selecione `REST Api` na tela inicial e então em <span style="color:#FF7043">Start From Scratch</span>.
5. Adicione os detalhes da API.
|    Name    | Context | Version |                    Endpoint                     |
|:----------:|:-------:|:-------:|:-----------------------------------------------:|
| HelloWorld | /hello  |  1.0.0  | http://run.mocky.io/v2/5185415ba171ea3a00704eed |

>[!note]
Usamos o protocolo HTTP porque para usar o HTTPS precisamos importar o certificado de <https://designer.mocky.io/> no WSO2 API-M.

6. Clique em  <span style="color:#65B891"></span><span style="color:#FF7043">Create & Publish</span>.
> Isso vai publicar nossa primeira API no `Developer Portal` assim como o deploy na `API Gateway`. Agora temos uma REST API com segurança OAuth 2.0 que está pronta para ser consumida.

### <span style="color:#FF7043"><b>Passo 2 - Se inscrevendo na API</b></span>

1. Acesse o `Developer Portal` <https://localhost:9443/devportal>
- A API `HelloWorld` publicada deve estar listada no portal.
2. Entre com as credenciais `admin/admin`
3. Clique na thumbnail da API para um overview da API.
4. <span style="color:#FF7043">Registre</span> uma aplicação OAuth 2.0
a. Clique em <span style="color:#FF7043">Subscriptions</span> no menu esquerdo.
b. Clique em <span style="color:#FF7043"><b>SUBSCRIPTION & KEY GENERATION WIZARD</b></span> em cima da página.

>[!note] 
>Os próximos 5 passos vão nos guiar para registrar uma aplicação OAuth 2.0, que será usada para consumir a API `HelloWorld`.

c. <span style="color:#FF7043">Crie</span> a aplicação OAuth 2.0.
Coloque o nome da aplicação, e clique em **Next** sem mudar nenhum dos valores padrão.
|    Application Name    | Per Token Quota |                   
|:----------:|:-------:|
| Greetings | 50PerMin  |

d. <span style="color:#FF7043">Inscreva</span> a aplicação na API.
Isso inscreve a aplicação `Greetings` na API `HelloWorld` no Bussiness Plan selecionado. Clique em **Next** sem modificar nenhum dos valores padrão.


e. <span style="color:#FF7043">Gere</span> as credenciais para a aplicação `Greetings`.
O `Grant Types` define vários protocolos, que serão permitidos pelo sistema, em que a aplicação será permitida para solicitar tokens. Clique em **Next**.

f. <span style="color:#FF7043">Gere</span> um token de acesso para a aplicação `Greetings` acessar a API `HelloWorld`
Esse passo permite que especificamos as permissões (`Scopes`) para o token. Clique em **Next** sem mudar nenhum dos valores padrão.

g. Clique no ícone de <span style="color:#FF7043">copiar</span> para copiar o token.

h. Clique em <span style="color:#FF7043">Finish</span>.
> Agora podemos testar nossa API `HelloWorld` com o token OAuth 2.0 que acabamos de gerar.

### <span style="color:#FF7043"><b>Passo 3 - Invoque a API</b></span>

Siga as instruções abaixo para invocar a API previamente criada com as chaves geradas.

1. Clique em <span style="color:#FF7043">Try it out</span> no menu esquerdo.
	Os recursos da API serão listados.
2. Copie o token de acesso no campo <span style="color:#FF7043">Acess Token</span>.

>[!info]
> Se for sua primeira vez usando o API test console no navegador, vá até a url [https://localhost:8243/](https://localhost:8243/). Isso vai solicitar a permissão de aceite do certificado usado pela API Gateway. 

3. Clique no recurso `GET` e clique em <span style="color:#FF7043">Try it out</span>.
4. Clique em <span style="color:#FF7043">Execute</span>.
	Deve aparecer `{"hello" : "world"}` como resposta.
É isso! Criamos com sucesso nossa primeira API, inscrevemos a ela pela aplicação OAuth 2.0, obtivemos um token de acesso para teste, e invocamos a API com o token.

### <span style="color:#4E878C"><b>Automatize o desenvolvimento e deployment da API</b></span>

> Veremos como usar <span style="color:#FF7043"><b>WSO2 API Controller (apictl)</b></span>, que é a ferramenta da linha de comando para mover APIs, API Products, e aplicações pelos ambientes WSO2 API-M e para performar operações de CI/CD.

### <span style="color:#ff8f43"><b>Antes de começar...</b></span>
1. Tenha certeza de que estamos rodando o WSO2 API Manager 4.2.0.
2. Baixe o `apictl`.
	a. Navegue até a [página do API Manager Tooling](https://wso2.com/api-management/tooling/).
	b. Baixe a versão 4.2.0 (ou a última versão da família 4.1.x) baseado no seu sistema operacional.
	c. Extraia o ZIP onde quiser.
		Será referida como o diretório `apictl`.
	d. Navegue até o diretório `apictl`.
	
>[!warning]
>**Se tiver usado uma versão anterior do apictl**, faça backup e remova o diretório `/home/<user>/.wso2apictl`

Opcionalmente, execute o seguinte comando para ver as operações disponíveis `./apictl --help`

3. Aponte o apictl para a instância do WSO2 API-M que você quer implantar APIs.

>[!note]
>Estamos assumindo que o WSO2 API-M está rodando localmente utilizando as portas padrão

Execute o seguinte comando para adicionar o ambiente para esse propósito:

``` cmd
./apictl add env dev \ --apim https://localhost:9443
```

> [!success] Ao executar o comando com sucesso, devemos receber a mensagem
> Default token endpoint 'https://localhost:9443/oauth2/token' is added as the token endpoint 
> Successfully added environment 'dev'

#### <span style="color:#FF7043"><b>Passo 1 - Criando uma API</b></span>

1. <span style="color:#FF7043">Inicialize</span> um projeto dando um nome à ele.
Vamos utilizar o comando abaixo para criar uma API chamada `PetstoreAPI`. Isso cria uma pasta chamada `PetstoreAPI` no diretório atual.
`./apictl init PetstoreAPI --oas https://apim.docs.wso2.com/en/4.2.0/assets/attachments/get_started/petstore.json`

>[!success] Ao executar o comando com sucesso, devemos receber a mensagem
> Initializing a new WSO2 API Manager project in <your-directory-path\>/PetstoreAPI
> Project initialized
> Open README file to learn more

> [!tip] 
> Opcionalmente, podemos utilizar o seguinte comando para visualizar as várias opções relacionadas a inicialização de um projeto
> `./apictl init --help`

2. <span style="color:#FF7043">Atualize</span> o arquivo `api.yaml`
	a. Abra e explore a pasta `PetstoreAPI` em uma IDE.
	b. Abra o arquivo `api.yaml`.
	c. Mude os valores dos seguintes atributos mostrados abaixo e salve:
	``` yaml
	lifeCycleStatus: PUBLISHED
	```
	``` yaml
	production_endpoints: 
		url: https://petstore.swagger.io/v2 
	sandbox_endpoints: 
		url: https://petstore.swagger.io/v2
	```
>[!note] 
> - Tenha certeza de que não há espaços entre o valor `context` no arquivo `api.yaml`
> - Mudar o `lifeCycleStatus` para `PUBLISHED` vai implantar (deploy) a API diretamente ao Developer Portal e API Gateway quando subirmos a API para o WSO2 API-M no próximo passo.
> - Se quisermos subir a API somente para o Publisher Portal, o status deve ser `CREATED`.

#### <span style="color:#FF7043"><b>Passo 2- Publique a API</b></span>

1. <span style="color:#FF7043">Suba</span> a API para o WSO2 API-M.
	Navegue até o diretório `apictl` e execute o seguinte comando:
	```yaml
	./apictl import api --file ./PetstoreAPI --environment dev
	```
> [!info] 
> As credenciais padrão são `admin/admin`.

2. Navegue pelo Publisher e Developer Portals para ver os detalhes da API.

#### <span style="color:#FF7043"><b>Passo 3 - Invoque a API</b></span>
 	
> [!error] 
> Invocar a API não funcionou!

1. <span style="color:#FF7043">Gere</span> o token de acesso usando `apictl`.
	Navegue novamente para o diretório e execute o seguinte comando:
	``` yaml
	./apictl get keys -e dev -n SwaggerPetstore -v 1.0.0 -r admin
	```
	Agora teremos um token de acesso que será usado para invocar nossa API.

> [!info] 
> Para mais informações sobre gerar chaves usando `apictl`, veja [Get keys for an API/API Product](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/setup/api-controller/ci-cd-with-wso2-api-management/#g-get-keys-for-an-apiapi-product).

2. <span style="color:#FF7043">Invoque</span> a API.
	Execute o seguinte cURL comando para invocar o `GET /pet` da API.
	Tenha certeza de enviar o token de acesso gerado anteriormente como o `Bearer` na requisição.

	```cmd
	curl -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUz....RWrACAUNSFBpxz1lRLqFlDiaVJAg" https://localhost:8243/SwaggerPetstore/1.0.0/pet/2 -k
	```
	Iremos receber a seguinte resposta:
	``` cmd
	{"id":2,"category":{"id":0,"name":"Dogs"},"name":"Crunch","photoUrls":["-"],"tags":[{"id":3,"name":"Crunchiogo"}],"status":"unavailable"}
	```