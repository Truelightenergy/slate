# Authentication

All API requests require an API token in the header. The token will be provided
by the Truelight team.

```shell
curl -X GET -H "Authorization: Token token=MY_TRUELIGHT_TOKEN" -H "Cache-Control:
no-cache" "https://app.truelightenergy.com/api/accounts"
```

> Make sure to replace `MY_TRUELIGHT_TOKEN` with your API token.

Truelight expects for the API key to be included in all API requests to the
server in a header that looks like the following:

`Authorization: Token token=MY_TRUELIGHT_TOKEN`

<aside class="notice">
You must replace <code>MY_TRUELIGHT_TOKEN</code> with your personal API token.
</aside>
