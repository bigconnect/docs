# PGP Signer

## PGP Signer Processor

The PGP Signer processor is an essential component for ensuring the integrity and authenticity of messages within a pipeline. By digitally signing messages, it allows recipients to verify the origin and integrity of the received message through a public PGP key. This process enhances security, providing assurance that the message has not been tampered with during transit. The PGP Signer processor offers support for various signing methods, each with distinct characteristics and use cases:

### Signing Methods

#### 1. Compressed Signature

This method compresses the message and generates a hash of the contents before signing. The message is encapsulated within the signature itself. As a result, the signature serves as a substitute for the message. This approach commonly yields a signature in printable ASCII format, also known as ASCII Armor output. You have the option to disable compression, resulting in a binary signature instead.

#### 2. Clear Signed

Clear signing maintains the message in its original form while appending the signature beneath the unaltered message text. This method allows for easy verification of both the message signature and its contents. The output in this case is categorized as application/pgp-signature, facilitating straightforward validation by recipients.

#### 3. Detached Signature with MIME

In this approach, the PGP Signer processor creates a multipart MIME document. The original message remains in clear text, while the signature is provided as an attached MIME part. This format is particularly suitable when the message content needs to be retained in its original state, separate from the signature.

Each of these methods provides distinct advantages depending on the specific requirements and desired outcomes for message security. By utilizing the PGP Signer processor, organizations can ensure reliable verification and bolster the overall trustworthiness of their communications.

### Configuring the PGP Signer <a href="#ariaid-title2" id="ariaid-title2"></a>

| General Property   | Description                                                                                                                                             |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Select Private Key | select a PGP key pair configured in the Certificate Store                                                                                               |
| Private Key Alias  | Enter the alias name of the PGP private key used in the Certificate Store                                                                               |
| Private Key Field  | Enter an expression that specifies the name of the message field that contains the private key                                                          |
| Signing Method     | Select the signing method that was used to create the digital signature for the message attachment: Compressed, Clear Signed, Detached Signature (MIME) |

