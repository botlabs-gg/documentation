# Authentication

<aside class="notice">
<b>To get access to our API, write an email to 
<a target="_blank" href="mailto:devs@weeb.sh">devs@weeb.sh</a>,
 which contains at least the following information: the name of your bot, it's guildcount, website (if any) and a link to a botlist site where the guildcount is visible</b>
</aside>


When calling our API, make sure to add an HTTP-Header with the name of `Authorization` and a value of `TokenType TOKEN`

Weeb.sh requires you to authenticate on most api calls, depending on when you signed up, you may have gotten one of the following tokens:

Bearer token: ```eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImlhdCI6MTUxNjIzOTAyMn0.TCYt5XsITJX1CxPCT8yAV-TVkIEq_PbChOMqsLfRoPsnsgw5WEuts01mq-pQy7UJiN5mgRxD-WUcX16dUEMGlv50aqzpqh4Qktb3rk-BuQy72IFLOqV0G_zS245-kronKb78cPN25DGlcTwLtjPAYuNzVBAh4vGHSrQyHUdBBPM```

Wolke token: ```SDE1STMzd0wtOjBlNjYyYTRmZjg2YWE1OGQwM2FlOTA1YzBhM2FlZGQ1NzZhZjVmMTk2NzE2YWUxNWVjZTVjZWY5```

<aside class="notice">
All new users get a wolke token. The listed api tokens are not working and just serve as examples, you have to replace them with your personal token.
</aside>
## Bearer token

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImlhdCI6MTUxNjIzOTAyMn0.TCYt5XsITJX1CxPCT8yAV-TVkIEq_PbChOMqsLfRoPsnsgw5WEuts01mq-pQy7UJiN5mgRxD-WUcX16dUEMGlv50aqzpqh4Qktb3rk-BuQy72IFLOqV0G_zS245-kronKb78cPN25DGlcTwLtjPAYuNzVBAh4vGHSrQyHUdBBPM"
```

For a bearer token the value of the `Authorization` header would look like this:

```Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImlhdCI6MTUxNjIzOTAyMn0.TCYt5XsITJX1CxPCT8yAV-TVkIEq_PbChOMqsLfRoPsnsgw5WEuts01mq-pQy7UJiN5mgRxD-WUcX16dUEMGlv50aqzpqh4Qktb3rk-BuQy72IFLOqV0G_zS245-kronKb78cPN25DGlcTwLtjPAYuNzVBAh4vGHSrQyHUdBBPM```

## Wolke token

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Wolke SDE1STMzd0wtOjBlNjYyYTRmZjg2YWE1OGQwM2FlOTA1YzBhM2FlZGQ1NzZhZjVmMTk2NzE2YWUxNWVjZTVjZWY5"
```

For a wolke token the value of the `Authorization` header would look like this:

```Wolke SDE1STMzd0wtOjBlNjYyYTRmZjg2YWE1OGQwM2FlOTA1YzBhM2FlZGQ1NzZhZjVmMTk2NzE2YWUxNWVjZTVjZWY5```
