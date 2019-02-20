## Overview

PKI setup for Mongo Client certificates

## Setup

Use the `init-pki` command to created the directory structure

Then create a certificate request 

```
easyrsa gen-req mongoclient

Using SSL: openssl OpenSSL 1.1.0g  2 Nov 2017
Generating a 2048 bit RSA private key
..+++
...............................................................................+++
writing new private key to '/home/fiona/wd/dwp/cis/enterprise_mongo_client/pki/private/mongoclient.key.lfNnD8JYZx'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Common Name (eg: your user, host, or client name) [mongoclient]:

Keypair and certificate request completed. Your files are:
req: /home/fiona/wd/dwp/cis/enterprise_mongo_client/pki/reqs/mongoclient.req
key: /home/fiona/wd/dwp/cis/enterprise_mongo_client/pki/private/mongoclient.key

``` 

Remove the passphrase so that the project can be shared

```
openssl rsa -in mongoclient.key -out mongoclient-unenc.key
```

Concatenate the key and issues certificate

```
cat pki/private/mongoclient.key ../enterprise_mongo_ca/pki/issued/mongoclient.crt > mongoclient-key-crt.pem
```


## Connecting to the server

Run the following command

```
mongo --ssl --port 27717 --host mongoserver admin --verbose --sslCAFile ../enterprise_mongo_ca/pki/ca.crt --sslPEMKeyFile mongoclient-key-crt.pem
```
