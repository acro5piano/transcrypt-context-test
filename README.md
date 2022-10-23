# transcrypt-context-test

Test of the new feature of transcrypt

# Command Log

## Initialize secrets

```sh
curl https://raw.githubusercontent.com/elasticdog/transcrypt/multiple-passphrases-in-one-repo-wip/transcrypt > transcrypt
./transcrypt

echo 'secret/* filter=crypt diff=crypt merge=crypt' >> .gitattributes
echo my-secret > secret/test
git commit -m 'add normal secret'

echo 'top-secret/* filter=crypt-super diff=crypt-super merge=crypt-super' >> .gitattributes
echo top-secret > top-secret/test
git commit -m 'add non-crypted top-secret'

./transcrypt --context=super
git commit -m 'add "super" context'
```

## Confirm password

```sh
transcrypt -d

The current repository was configured using transcrypt version 2.3.0-pre
and has the following configuration:

  GIT_WORK_TREE:  /home/kazuya/ghq/github.com/acro5piano/transcrypt-context-test
  GIT_DIR:        /home/kazuya/ghq/github.com/acro5piano/transcrypt-context-test/.git
  GIT_ATTRIBUTES: /home/kazuya/ghq/github.com/acro5piano/transcrypt-context-test/.gitattributes

  CONTEXT:  default
  CIPHER:   aes-256-cbc
  PASSWORD: 2JNNqV79pjk+klOYugPhV7woSOMxylaWd8NcgoiN

The repository has 2 contexts: default super

Copy and paste the following command to initialize a cloned repository:

  transcrypt -c aes-256-cbc -p '2JNNqV79pjk+klOYugPhV7woSOMxylaWd8NcgoiN'
```

```sh
transcrypt -d --context super

The current repository was configured using transcrypt version 2.3.0-pre
and has the following configuration for context 'super':

  GIT_WORK_TREE:  /home/kazuya/ghq/github.com/acro5piano/transcrypt-context-test
  GIT_DIR:        /home/kazuya/ghq/github.com/acro5piano/transcrypt-context-test/.git
  GIT_ATTRIBUTES: /home/kazuya/ghq/github.com/acro5piano/transcrypt-context-test/.gitattributes

  CONTEXT:  super
  CIPHER:   aes-256-cbc
  PASSWORD: EjV+iIIU4lTqVn51KzfvBtF/LoSUD5D+S+nUmY7u

The repository has 2 contexts: default super

Copy and paste the following command to initialize a cloned repository for context 'super':

  transcrypt -C super -c aes-256-cbc -p 'EjV+iIIU4lTqVn51KzfvBtF/LoSUD5D+S+nUmY7u'
```

## Decrypt repo on another directory

```sh
git clone git@github.com:acro5piano/transcrypt-context-test.git
cd transcrypt-context-test

cat secret/test
# => U2FsdGVkX1+o4ir0hrAfeEBKnlyEHzTBPy2kWQBqZak=

./transcrypt -c aes-256-cbc -p '2JNNqV79pjk+klOYugPhV7woSOMxylaWd8NcgoiN'

cat secret/test
# => my-secret

# Confirm still encrypted
cat top-secret/test
# => U2FsdGVkX18DKOB9Jisu->9mnt43V0OVJINprOouJwU=

./transcrypt -C super -c aes-256-cbc -p 'EjV+iIIU4lTqVn51KzfvBtF/LoSUD5D+S+nUmY7u'

cat top-secret/test
# => top-secret
```
