# PGP Validator

You can utilize the PGP Validator filter to verify messages signed with Pretty Good Privacy (PGP) as they pass through the system pipeline. This verification process is accomplished by ensuring the validity of the message signature against the signer’s public PGP key. The PGP Validator filter plays an essential role in maintaining the integrity and authenticity of communications by providing a reliable method to confirm that a message has not been altered and genuinely originates from the purported sender.

To achieve thorough verification, the PGP Validator supports a range of signing methods. These include traditional PGP signatures, which rely on strong cryptographic principles to secure the message content. By leveraging the public key infrastructure, PGP ensures that only the holder of a corresponding private key could have generated the signature, thereby offering assurance of authenticity.

Integrating PGP Validator into your communication flow not only enhances security but also upholds data protection standards. This is especially critical in environments where sensitive information is exchanged, and the legitimacy of messages is paramount. By adopting PGP Validator for signature verification, users can maintain trust and ensure all communications are conducted with the highest level of security assurance.

* Compressed: Verifies a compressed signature. Because the message is contained in the signature, this signature is used in place of the message.
* Clear Signed: A clear signed message has the message intact and a signature attached under the clear message text. Verifying this message verifies the sender and the message integrity.
* Detached Signature (MIME): Verifies a multipart MIME document where the message is in clear text and the signature is attached as a MIME part.

An example use case for this processor would be when files are sent over Secure Shell File Transfer Protocol (SFTP) in PGP signed format. PGP Validator can be used to verify the message, and. The clean files can be PGP signed again using the PGP Signer processor before being sent over SFTP to their target destination.

### Configuring the PGP Validator <a href="#ariaid-title2" id="ariaid-title2"></a>

| General Property  | Description                                                                                                                                             |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Select Public Key | select a PGP key pair configured in the Certificate Store                                                                                               |
| Public Key Alias  | Enter the alias name of the PGP public key used in the Certificate Store                                                                                |
| Public Key Field  | Enter an expression that specifies the name of the message field that contains the public key                                                           |
| Signing Method    | Select the signing method that was used to create the digital signature for the message attachment: Compressed, Clear Signed, Detached Signature (MIME) |

