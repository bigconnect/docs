# Using OAuth2 to Authorize Users

You can create applications that access Liferay’s headless REST APIs using the OAuth 2.0 authorization protocol. The provided sample React app demonstrates three different OAuth2 token-based authentication flows: authorization code flow, client credentials flow, and password flow. For more details on the OAuth2 Administration panel see [Creating an OAuth2 Application](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/creating-oauth2-applications)

### [Set Up Liferay DXP](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/using-oauth2-to-authorize-users#set-up-liferay-dxp) <a href="#set-up-liferay-dxp" id="set-up-liferay-dxp"></a>

Start a new Liferay DXP instance by running

```bash
docker run -it -m 8g -p 8080:8080 liferay/dxp:2024.q1.1
```

Sign in to Liferay at [http://localhost:8080](http://localhost:8080/) using the email address _test@liferay.com_ and the password _test_. When prompted, change the password to _learn_.

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and navigate to _Control Panel_ → _Security_ → _OAuth 2 Administration_.
2. Click _Add_ (![Add Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to create a new OAuth2 application.
3.  Give the application a name (e.g., foo). Set the Website URL as `http://localhost:3000` and the Callback URI as `http://localhost:3000/grant-type-authorization-code`. Click _Save_.

    ![Fill out the form to the create a new OAuth2 application.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/using-oauth2/using-oauth2-to-authorize-users/images/01.png)
4.  Copy the Client ID and Client Secret to your clipboard. To get the Client Secret click _Edit_. Copy the value from the pop-up window.

    These values are needed later in the sample React app.
5.  Click the _Scopes_ tab at the top of the page. Scroll down, click _LIFERAY.HEADLESS.ADMIN.USER_, then check the box for _read data on your behalf_.

    ![Enable read privileges for LIFERAY.HEADLESS.ADMIN.USER.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/using-oauth2/using-oauth2-to-authorize-users/images/02.png)

    Click _Save_. Your OAuth2 Application now has read privileges for the Admin User API category.
6. Next, open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_ tab, and go to _System Settings_ → _Security Tools_.
7. Go to the _Portal Cross-Origin Resource Sharing (CORS)_ tab and click _Default Portal CORS Configuration_.
8. Add a _URL Pattern_ with the value `/o/headless-admin-user/*` and click _Save_. This enables CORS for the `headless-admin-user` category of APIs.

### [Deploy the Sample React App](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/using-oauth2-to-authorize-users#deploy-the-sample-react-app) <a href="#deploy-the-sample-react-app" id="deploy-the-sample-react-app"></a>

1.  Download and unzip the [OAuth2 React App](https://resources.learn.liferay.com/dxp/latest/en/headless-delivery/using-oauth2/liferay-c2b6.zip).

    ```bash
    curl https://resources.learn.liferay.com/dxp/latest/en/headless-delivery/using-oauth2/liferay-c2b6.zip -O
    ```

    ```bash
    unzip liferay-c2b6.zip
    ```

    ```bash
    cd liferay-c2b6
    ```
2.  Verify that `node` and `yarn` are installed. If not, run the setup script and follow the prompts:

    ```bash
    ./setup_tutorial.sh
    ```
3.  Navigate into the app’s root directory and start the React server.

    ```bash
    cd c2b6-custom-element
    ```

    ```bash
    yarn install && yarn start
    ```

#### [Authorization Code Flow](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/using-oauth2-to-authorize-users#authorization-code-flow) <a href="#authorization-code-flow" id="authorization-code-flow"></a>

The authorization code flow requires users to log in with their credentials and approve authorization before permission is granted to the app. This additional step is avoided with the other flows.

1. Open the React app running at [http://localhost:3000](http://localhost:3000/), then click _Authorization Code Flow_ at the top of the page.
2. Enter `http://localhost:8080/o/oauth2/authorize` for the Liferay authorize URL. Paste the client ID from your clipboard. Click _Authorize_.
3.  If you’re not already logged in, you are redirected to the Liferay login page before being sent to the authorization page. Enter your username and password (e.g. _test@liferay.com:learn_) and click _Sign In_. On the authorization page, click _Authorize_. If you are already logged in, you are sent directly to the authorization page.

    ![Click Authorizez to authorize the application.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/using-oauth2/using-oauth2-to-authorize-users/images/03.png)

    You are redirected back to the React app. Note the authorization code that is used to make the next API call appears.
4. Enter `http://localhost:8080/o/oauth2/token` for the Liferay token URL. Paste the client ID and client secret from your clipboard. Click _Get Token_. Note the authorization token that can be used to make future REST API calls appears.
5.  Enter `http://localhost:8080/o/headless-admin-user/v1.0/user-accounts` for the Liferay get user URL. Click _Get Data_. The React app makes a REST API call to Liferay using token-based authentication and returns a list of Liferay users.

    ![A list of Liferay users appears.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/using-oauth2/using-oauth2-to-authorize-users/images/04.png)

#### [Client Credentials Flow](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/using-oauth2-to-authorize-users#client-credentials-flow) <a href="#client-credentials-flow" id="client-credentials-flow"></a>

The client credentials flow is used typically for server to server interaction and does not involve any users.

1. Open the React app running at [http://localhost:3000](http://localhost:3000/), then click _Client Credentials Flow_ at the top of the page.
2. Enter `http://localhost:8080/o/oauth2/token` for the Liferay token URL. Paste the client ID and client secret from your clipboard. Click _Get Token_. Note the authorization token that can be used to make future REST API calls appears.
3. Enter `http://localhost:8080/o/headless-admin-user/v1.0/user-accounts` for the Liferay get user URL. Click _Get Data_. The React app makes a REST API call to Liferay using token-based authentication and returns a list of Liferay users.

#### [Password Flow](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/using-oauth2-to-authorize-users#password-flow) <a href="#password-flow" id="password-flow"></a>

In the password flow authorization, the React app passes the username and password directly in the request.

WARNING

In a password flow, the username and password are directly exposed to the application. Thus the user must trust the application. Passing the username and password in the API request also carries risk. Using password flow is not recommended.

1. Open the React app running at [http://localhost:3000](http://localhost:3000/), then click _Password Flow_ at the top of the page.
2. Enter `http://localhost:8080/o/oauth2/token` for the Liferay token URL.
3. Paste the client ID and client secret from your clipboard.
4. Enter your username and password (e.g. _test@liferay.com:learn_).
5. Click _Get Token_. Note the authorization token that can be used to make future REST API calls appears.
6. Enter `http://localhost:8080/o/headless-admin-user/v1.0/user-accounts` for the Liferay get user URL. Click _Get Data_. The React app makes a REST API call to Liferay using token-based authentication and returns a list of Liferay users.

### [Examine the Code](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/using-oauth2-to-authorize-users#examine-the-code) <a href="#examine-the-code" id="examine-the-code"></a>

The React app’s `components` folder defines the UI elements and logic for handling the events fired by clicking buttons. For example, the `Authorize.js` file redirects to the authorization page, `Token.js` fetches the access token, and `Users.js` fetches the list of users. Each authorization flow uses some or all of these components.

The API request to get an authorization token and the GET request is defined in the `Requests.js` file in the `utils` folder.

```log
src
├── components
│   ├── Authorize.js
│   ├── Token.js
│   ├── Users.js
├── routes
│   ├── grant-type-authorization-code
│   |   ├── AuthorizationCode.js
│   ├── grant-type-client-credentials
│   |   ├── ClientCredentials.js
│   ├── grant-type-password
│   |   ├── Password.js
├── utils
│   ├── Requests.js
├── App.js
└── index.js
```

#### [Authorization Grant Type](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/using-oauth2-to-authorize-users#authorization-grant-type) <a href="#authorization-grant-type" id="authorization-grant-type"></a>

The `AuthorizationCode.js` file of the `grant-type-authorization-code` flow uses all three components (`Authorize.js`, `Token.js`, and `Users.js`)

```javascript
<h1>Authorization Code Flow</h1>

Callback URI: http://localhost:3000/grant-type-authorization-code (or wherever the React app is running)

<br />

Scope: Headless Admin User (e.g. Liferay.Headless.Admin.User.everything.read)

<br />

CORS: Configure Portal CORS URL Pattern (e.g. /o/headless-admin-user/*)

<Authorize />

<Token
	grantType='authorization_code'
	handleToken={handleToken}
/>

<Users token={token} />
```

In the code, the `Authorize.js` component handles the first step. A request for authorization is made when the _Authorize_ button is clicked.

```javascript
function Authorize() {
	const [authUrl, setAuthUrl] = React.useState('');
	const [clientId, setClientId] = React.useState('');

	function handleAuthorize(event) {
		event.preventDefault();

		try {
			window.location.replace(
				authUrl + '?response_type=code&client_id=' + clientId
			);
		}
		catch (error) {
			throw new Error(error);
		}
	}

	const urlSearchParams = new URLSearchParams(window.location.search);

	var code = urlSearchParams.get('code');

	return (
		<div>
			<h2>Authorize</h2>

			<input
				onChange={(event) => setAuthUrl(event.target.value)}
				placeholder="Liferay Authorize URL"
				style={{width: 500}}
				type="text"
				value={authUrl}
			/>

			(e.g. http://localhost:8080/o/oauth2/authorize)

			<br />

			<input
				onChange={(event) => setClientId(event.target.value)}
				placeholder="Client ID"
				style={{width: 500}}
				type="text"
				value={clientId}
			/>

			<br />

			<form onSubmit={handleAuthorize}>
				<button type='onSubmit'>Authorize</button>
			</form>

			{code && (
				<div>
					<br />

					Authorization Code:

					<br />

					{code}
				</div>
			)}
		</div>
	);
}
```

Along with the redirect, a one-time authorization code is passed back to the app in the URL (e.g. `http://localhost:3000/grant-type-authorization-code?code={code}`).

The `Token.js` component handles the next step. Clicking _Get Token_ calls the `getAuthToken` function in the `Requests.js` file.

```javascript
import {getAuthToken} from '../utils/Requests';

function Token({handleToken, grantType}) {
	const [clientId, setClientId] = React.useState('');
	const [clientSecret, setClientSecret] = React.useState('');
	const [password, setPassword] = React.useState('');
	const [token, setToken] = React.useState('');
	const [tokenUrl, setTokenUrl] = React.useState('');
	const [username, setUsername] = React.useState('');

	async function handleGetToken() {
		const token = await getAuthToken({clientId, clientSecret, grantType, password, tokenUrl, username});

		handleToken(token);

		setToken(token);
	}

	return (
		<div className='Token'>
			<h2>Get Token</h2>

			<input
				onChange={(event) => setTokenUrl(event.target.value)}
				placeholder="Liferay Token URL"
				style={{width: 500}}
				type="text"
				value={tokenUrl}
			/>

			(e.g. http://localhost:8080/o/oauth2/token)

			<br />

			<input
				onChange={(event) => setClientId(event.target.value)}
				placeholder="Client ID"
				style={{width: 500}}
				type="text"
				value={clientId}
			/>

			<br />

			<input
				onChange={(event) => setClientSecret(event.target.value)}
				placeholder="Client Secret"
				style={{width: 500}}
				type="text"
				value={clientSecret}
			/>

			<br />

			{grantType === 'password' && (
				<div>
					<input
						onChange={(client) => setUsername(client.target.value)}
						placeholder="User Name"
						style={{width: 500}}
						type="text"
						value={username}
					/>

					<br />

					<input
						onChange={(client) =>
							setPassword(client.target.value)
						}
						placeholder="User Password"
						style={{width: 500}}
						type="text"
						value={password}
					/>

					<br />
				</div>
			)}

			<button onClick={handleGetToken}>Get Token</button>

			{token && (
				<div>
					<br />

					Authorization Token:

					<br />

					{token.access_token}
				</div>
			)}
		</div>
	);
}
```

In the `Token.js` component, the parameters `client_id`, `client_secret`, `code`, `grant_type`, and `redirect_uri` are sent in this API request. If the parameters are valid, Liferay returns a JSON response containing the access token.

Example response:

```json
{
  "access_token": "2fda85abec524112dae612d35e9f9abd71650d364dee47c645b7574c6bffe91",
  "token_type": "Bearer",
  "expires_in": 600,
  "scope": "Liferay.Headless.Admin.User.everything.read"
}
```

The `Users.js` component parses the response for the `access_token`.

Finally, clicking _Get Data_ calls the `getUsers` function in the `Requests.js` file.

#### [Client Credentials Grant Type](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/using-oauth2-to-authorize-users#client-credentials-grant-type) <a href="#client-credentials-grant-type" id="client-credentials-grant-type"></a>

The `ClientCredentials.js` file of the `grant-type-client-credentials` flow uses two components (i.e. `Token.js` and `Users.js`).

```javascript
function ClientCredentials() {
	const [token, setToken] = React.useState({});

	function handleToken(token) {
		setToken(token);
	}

	return (
		<div>
			<h1>Client Credentials Code Flow</h1>

			Scope: Headless Admin User (e.g. Liferay.Headless.Admin.User.everything.read)

			<br />

			CORS: Configure Portal CORS URL Pattern (e.g. /o/headless-admin-user/*)

			<Token
				grantType='client_credentials'
				handleToken={handleToken}
			/>

			<Users token={token} />
		</div>
	);
}
```

In the `Token.js` component, the parameters `client_id`, `client_secret`, and `grant_type` are sent in the API request. If the parameters are valid, Liferay returns a JSON response containing the access token.

The `Users.js` component parses the response for the `access_token`.

Finally, clicking _Get Data_ calls the `getUsers` function in the `Requests.js` file.

#### [Password Grant Type](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/using-oauth2-to-authorize-users#password-grant-type) <a href="#password-grant-type" id="password-grant-type"></a>

The `Password.js` file of the `grant-type-password` flow uses two components (i.e. `Token.js` and `Users.js`).

```javascript
function Password() {
	const [token, setToken] = React.useState({});

	function handleToken(token) {
		setToken(token);
	}

	return (
		<div>
			<h1>Password Flow</h1>

			Scope: Headless Admin User (e.g. Liferay.Headless.Admin.User.everything.read)

			<br />

			CORS: Configure Portal CORS URL Pattern (e.g. /o/headless-admin-user/*)

			<Token
				grantType='password'
				handleToken={handleToken}
			/>

			<Users token={token} />
		</div>
	);
}
```

In the `Token.js` component, the parameters `client_id`, `client_secret`, `grant_type`, `password`, and `username` are sent as parameters in the API request. If the parameters are valid, the Liferay server returns a JSON response containing the access token.

The `Users.js` component parses the response for the `access_token`.

Finally, clicking _Get Data_ calls the `getUsers` function in the `Requests.js` file.

\
