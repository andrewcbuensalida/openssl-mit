# openssl
https://classroom.emeritus.org/courses/8898/pages/openssl-library?module_item_id=1486479

# To generate private key pem file
download openssl from https://slproweb.com/products/Win32OpenSSL.html
open win64 openssl command prompt
`openssl genrsa -out myprivate.pem 512`

# To generate public key from that private key
  `openssl rsa -in myprivate.pem -pubout > mypublic.pem`

# To sign a document meaning encrypt hello.txt and output sha1.sign (signature)
`openssl dgst -sha1 -sign myprivate.pem -out sha1.sign hello.txt`

# To verify if file was sent from me. 
when friend has signature (sha1.sign) and mypublic.pem and hello.txt, they can verify if hello.txt came from me by
`openssl dgst -sha1 -verify mypublic.pem -signature sha1.sign hello.txt`
this is output verified ok

# for friend To send an encrypted hello.txt file to me
repeat generate private and public key for friend
`openssl genrsa -out friend.pem 512`
`openssl rsa -in friend.pem -pubout > friendpublic.pem`
friend encrypts hello.txt
`openssl rsautl -encrypt -pubin -inkey friendpublic.pem -ssl -in hello.txt -out encrypthello.txt` <--rsautl is deprecated in version 3 so try this
`openssl pkeyutl -encrypt -pubin -inkey friendpublic.pem -in hello.txt -out encrypthello.txt`
Then to decrypt encrypthello.txt
`openssl pkeyutl -decrypt -inkey friend.pem -in encrypthello.txt -out myDecryptedMessage.txt`