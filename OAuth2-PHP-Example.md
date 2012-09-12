Here is a complete PHP sample using [adoy's PHP-OAuth2 library](https://github.com/adoy/PHP-OAuth2) written by [/u/Pathogen-David](http://www.reddit.com/user/Pathogen-David/).

It will authenticate the user and print out the results of their me.json:

    <?php
    if (isset($_GET["error"]))
    {
        echo("<pre>OAuth Error: " . $_GET["error"]."\n");
        echo('<a href="index.php">Retry</a></pre>');
        die;
    }

    $authorizeUrl = 'https://oauth.reddit.com/api/v1/authorize';
    $accessTokenUrl = 'https://oauth.reddit.com/api/v1/access_token';
    $clientId = 'CLIENT_ID';
    $clientSecret = 'CLIENT_SECRET';

    $redirectUrl = "REDIRECT_URI";

    require("Client.php");
    require("GrantType/IGrantType.php");
    require("GrantType/AuthorizationCode.php");

    $client = new OAuth2\Client($clientId, $clientSecret, OAuth2\Client::AUTH_TYPE_AUTHORIZATION_BASIC);

    if (!isset($_GET["code"]))
    {
        $authUrl = $client->getAuthenticationUrl($authorizeUrl, $redirectUrl, array("scope" => "identity", "state" => "SomeUnguessableValue"));
        header("Location: ".$authUrl);
        die("Redirect");
    }
    else
    {
        $params = array("code" => $_GET["code"], "redirect_uri" => $redirectUrl);
        $response = $client->getAccessToken($accessTokenUrl, "authorization_code", $params);
        
        $accessTokenResult = $response["result"];
        $client->setAccessToken($accessTokenResult["access_token"]);
        $client->setAccessTokenType(OAuth2\Client::ACCESS_TOKEN_BEARER);
        
        $response = $client->fetch("https://oauth.reddit.com/api/v1/me.json");
        
        echo('<strong>Response for fetch me.json:</strong><pre>');
        print_r($response);
        echo('</pre>');
    }
    ?>

# Example Response:

**Response for fetch me.json:**

    Array
    (
        [result] => Array
            (
                [name] => Pathogen-David
                [created] => 1308703383
                [created_utc] => 1308703383
                [link_karma] => 2101
                [comment_karma] => 11483
                [is_gold] => 1
                [id] => 5eqe2
            )
    
        [code] => 200
        [content_type] => application/json; charset=UTF-8
    )