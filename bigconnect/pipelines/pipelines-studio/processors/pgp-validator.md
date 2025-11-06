# PGP Validator

You can utilize the PGP Validator filter to verify messages signed with Pretty Good Privacy (PGP) as they pass through the system pipeline. This verification process is accomplished by ensuring the validity of the message signature against the signer’s public PGP key. The PGP Validator filter plays an essential role in maintaining the integrity and authenticity of communications by providing a reliable method to confirm that a message has not been altered and genuinely originates from the purported sender.

To achieve thorough verification, the PGP Validator supports a range of signing methods. These include traditional PGP signatures, which rely on strong cryptographic principles to secure the message content. By leveraging the public key infrastructure, PGP ensures that only the holder of a corresponding private key could have generated the signature, thereby offering assurance of authenticity.

Integrating PGP Validator into your communication flow not only enhances security but also upholds data protection standards. This is especially critical in environments where sensitive information is exchanged, and the legitimacy of messages is paramount. By adopting PGP Validator for signature verification, users can maintain trust and ensure all communications are conducted with the highest level of security assurance.

### Signing Methods

#### 1. Compressed Signature

This method compresses the message and generates a hash of the contents before signing. The message is encapsulated within the signature itself. As a result, the signature serves as a substitute for the message. This approach commonly yields a signature in printable ASCII format, also known as ASCII Armor output. You have the option to disable compression, resulting in a binary signature instead.

#### 2. Clear Signed

Clear signing maintains the message in its original form while appending the signature beneath the unaltered message text. This method allows for easy verification of both the message signature and its contents. The output in this case is categorized as application/pgp-signature, facilitating straightforward validation by recipients.

#### 3. Detached Signature with MIME

In this approach, the PGP Signer processor creates a multipart MIME document. The original message remains in clear text, while the signature is provided as an attached MIME part. This format is particularly suitable when the message content needs to be retained in its original state, separate from the signature.

Each of these methods provides distinct advantages depending on the specific requirements and desired outcomes for message security. By utilizing the PGP Signer processor, organizations can ensure reliable verification and bolster the overall trustworthiness of their communications.

### Configuring the PGP Validator <a href="#ariaid-title2" id="ariaid-title2"></a>

| General Property  | Description                                                                                                                                             |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Select Public Key | select a PGP key pair configured in the Certificate Store                                                                                               |
| Public Key Alias  | Enter the alias name of the PGP public key used in the Certificate Store                                                                                |
| Public Key Field  | Enter an expression that specifies the name of the message field that contains the public key                                                           |
| Signing Method    | Select the signing method that was used to create the digital signature for the message attachment: Compressed, Clear Signed, Detached Signature (MIME) |

