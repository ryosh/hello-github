```
apt-get update
apt-get install softhsm2 libsofthsm2-dev pkcs11-dump
apt-get install openssl -y
softhsm2-util --init-token --slot 0 --label greengrass --so-pin 12345 --pin 1234
mkdir -p /greengrass/softhsm2/tokens
echo "directories.tokendir = /greengrass/softhsm2/tokens" > /etc/softhsm/softhsm2.conf
echo "objectstore.backend = file" >> /etc/softhsm/softhsm2.conf
softhsm2-util --init-token --slot 0 --label greengrass --so-pin 12345 --pin 1234
pkcs11-tool --list-objects --slot 1748834104 --module /usr/lib/softhsm/libsofthsm2.so -l
softhsm2-util --show-slots
pkcs11-tool --list-objects --slot 748138923 --module /usr/lib/softhsm/libsofthsm2.so -l
openssl genrsa 1024 > private-key.pem
cp private-key.pem hash.private.key
openssl pkcs8 -topk8 -inform PEM -outform PEM -nocrypt -in hash.private.key -out hash.private.pem
softhsm2-util --import hash.private.pem --slot 1748834104 --label iotkey --id 0000 --pin 1234
softhsm2-util --import hash.private.pem --slot 748138923 --label iotkey --id 0000 --pin 1234
pkcs11-tool --list-objects --slot 1748834104 --module /usr/lib/softhsm/libsofthsm2.so -l
pkcs11-tool --list-objects --slot 748138923 --module /usr/lib/softhsm/libsofthsm2.so -l
```
