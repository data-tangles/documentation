## API Interaction

phpIPAM provides a useful [API](https://phpipam.net/api/api_documentation/) for interacting with phpIPAM. This is very useful if you need to find free IP's in a subnet or perhaps write info to an entry as part of some automation.

It is advisable to first create a user within phpIPAM for use with the API. The authentication method uses an authorization HTTP header which you can then use in subsequent API calls.

## Return token

```
curl -k https://ipam.example.com/api/myphpipamappid/user/ -X POST -u <username>:<password>  
```

## Fetch token

```
curl -k https://ipam.example.com/api/myphpipamappid/sections/ --header "Content-Type: application/json" "token: .J1e9ipFZkPE6EvIRAqEf9hp" -X GET 
```

## Fetch next free address in subnet

```
curl -k https://ipam.example.com/api/myphpipamappid/addresses/first_free/11 --header "Content-Type: application/json" "token: .J1e9ipFZkPE6EvIRAqEf9hp" -X GET 
```

## Reserve free entry and add description

```
curl -k https://ipam.example.com/api/myphpipamappid/addresses/first_free/11 --header "Content-Type: application/json" "token: .J1e9ipFZkPE6EvIRAqEf9hp" --data '{"description":"App Server"}' -X PATCH
```