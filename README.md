# transcrypt-context-test

Test of the new feature of transcrypt

# Command Log

```sh
curl https://raw.githubusercontent.com/elasticdog/transcrypt/multiple-passphrases-in-one-repo-wip/transcrypt > transcrypt
./transcrypt

echo 'secret/* filter=crypt diff=crypt merge=crypt' >> .gitattributes
echo my-secret > secret/test
git commit -m 'add normal secret'

echo 'top-secret/* filter=crypt-super diff=crypt-super merge=crypt-super' >> .gitattributes
echo top-secret > top-secret/test
git commit -m 'add top secret'
```
