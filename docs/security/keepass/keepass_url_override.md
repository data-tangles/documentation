# KeePass URL Override

[KeePass](https://keepass.info) is a very useful Password Safe that runs on your machine however some of the built in URL overrides don't work as well. 

Below are some useful custom URL overrides you can implement for improved functionality within the Windows version.

[This](https://keepass.info/help/base/autourl.html) link will assist with the steps to implement the URL override. 

## Remote Desktop Protocol

```
cmd://cmd /c cmdkey /generic: TERMSRV/{URL:RMVSCM} /user:{USERNAME} /pass:"{PASSWORD}" && mstsc /v:{URL:RMVSCM} && timeout /t 5 /nobreak && cmdkey /delete:TERMSRV/{URL:RMVSCM}
```

## SSH (PuTTY)

```
cmd://"putty.exe" -ssh "{USERNAME}@{URL:HOST}" -pw "{PASSWORD}" 
```