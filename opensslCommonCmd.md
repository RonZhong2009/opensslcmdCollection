#### private key and csr being generated together, by default outform is  PEM
  openssl req -config openssl.cnf -newkey rsa:2048 -sha256 -nodes -out servercert.csr

  openssl req -config openssl.cnf -newkey rsa:2048 -sha256 -nodes -out servercert.csr -outform PEM

#### verify the csr
  openssl req -text -noout -verify -in servercert.csr  
  openssl req -in servercert.csr -text

#### check the private key
  openssl rsa -in enc-testsvr-enc.key -check  
  openssl rsa -in testcakey.pem -check

#### convert the pkcs8 into pem format
  openssl pkcs8 -inform DER -in testcakey.der -passin pass:passwordstring  -outform PEM -out testcakey.pem  
  openssl x509 -inform der -in testcakey.der -out testcakey.pem

#### view the certificate
  openssl x509 -in testcacert.der  -inform der -text  
  openssl ca -config openssl-ca.cnf -policy signing_policy -extensions signing_req -out testsvr-cert.der -infiles servercert.csr  -passout pass:passwordstring

#### der file can't be used as key file, only pem file or key file
  openssl ca -config openssl-ca.cnf -key testcakey.der -passin pass:passwordstring -policy signing_policy -extensions signing_req -out testsvr-cert.der -infiles servercert.csr  
  openssl ca -config openssl-ca.cnf -passin pass:passwordstring -policy signing_policy -extensions signing_req -out testsvr-cert.der -infiles servercert.csr  
 openssl ca -config openssl-ca.cnf -policy signing_policy -extensions signing_req -out testsvr-cert.pem -infiles servercert.csr 

#### convert the pem into pcks8
  openssl pkcs8 -topk8 -inform PEM -in testcakeypair.pem -outform DER -out testcakey.der -passout pass:2222passwordstr -v1 PBES2 -v2prf hmacWithSHA256 -iter 1000000  
  openssl pkcs8 -inform DER -in testcakey.der pass:passwordstring -outform PEM -out testcakey.pem

#### sign a certificate with CA
 openssl x509 -req -in servercert.csr -signkey testclientkey.key -CA testcacert.pem -CAkey testcakey.pem -CAcreateserial   -out testclient-cert.der  -days 365
