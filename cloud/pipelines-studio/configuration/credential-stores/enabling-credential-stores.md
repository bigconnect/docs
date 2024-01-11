# Enabling Credential Stores

You can configure BigConnect Pipelines to use one or more credential stores. Each credential store is identified by a unique credential store ID.

You specify the credential stores that BigConnect Pipelines can use in the $SDC\_CONF/credential-stores.properties file. The file includes the following information:\
\
**credentialStores property**\
\
This property defines the credential stores that BigConnect Pipelines can use.\
\
By default, the property is commented out and includes a default credential store ID for each of the supported credential store types, such as `aws` for AWS Secrets Manager and `azure` for Azure Key Vault.\
\
To enable using credential stores, you uncomment this property and enter a comma-separated list of the credential store IDs to use.\
\
You can specify multiple credential stores of the same type or of different types, such as two Hashicorp Vaults and one Java keystore. You simply specify a unique ID for each credential store.\
\
**Sets of related properties**\
\
Each supported credential store type has a set of related properties. The property names include the default credential store IDs originally specified in the `credentialStores` property.For example, the CyberArk properties include `cyberark`, the default CyberArk ID, in each CyberArk property name, such as `credentialStore.cyberark.config.region` and `credentialStore.cyberark.config.access.key`.When you use a custom credential store ID, you must update all related property names to match the custom ID. \
\
For example, if you want to use `cyberarkUS` as a custom ID, you must update all CyberArk default property names for the `cyberarkUS` credential store replacing `cyberark` with `cyberarkUS`.\
\
**Note**: When you want to use multiple credential stores of the same type, you must have a set of related store properties that are renamed and defined appropriately for each credential store.

For example, say you want to use two Azure credential stores, `azureDev` for development and `azureProd` for production. To do this, you specify the credential store IDs in the `credentialStores` property and make a copy of the related Azure credential store properties, so you have one set for each credential store.

Then, you rename and configure the properties for `azureDev`, and you do the same for `azureProd`. The resulting properties might look as follows, with important changes highlighted:

<pre><code>################################################
#      Data Collector Credential Stores        #
################################################

<strong>credentialStores=azureDev,azureProd
</strong>
############################################################
<strong># azureDev: Azure Key Vault Credential Store Configuration #
</strong>############################################################

<strong>credentialStore.azureDev.def=streamsets-datacollector-azure-keyvault-credentialstore-lib::com_streamsets_datacollector_credential_azure_keyvault_AzureKeyVaultCredentialStore
</strong><strong>credentialStore.azureDev.config.credential.refresh.millis=30000
</strong><strong>credentialStore.azureDev.config.credential.retry.millis=15000
</strong><strong>credentialStore.azureDev.config.vault.url=https://development.vault.azure.net/
</strong><strong>credentialStore.azureDev.config.client.id=devClientID
</strong><strong>credentialStore.azureDev.config.client.key=devClientKey
</strong><strong>credentialStore.azureDev.config.enforceEntryGroup=false
</strong>
#############################################################
<strong># azureProd: Azure Key Vault Credential Store Configuration #
</strong>#############################################################

<strong>credentialStore.azureProd.def=streamsets-datacollector-azure-keyvault-credentialstore-lib::com_streamsets_datacollector_credential_azure_keyvault_AzureKeyVaultCredentialStore
</strong><strong>credentialStore.azureProd.config.credential.refresh.millis=30000
</strong><strong>credentialStore.azureProd.config.credential.retry.millis=15000
</strong><strong>credentialStore.azureProd.config.vault.url=https://production.vault.azure.net/
</strong><strong>credentialStore.azureProd.config.client.id=prodClientID
</strong><strong>credentialStore.azureProd.config.client.key=prodClientKey
</strong><strong>credentialStore.azureProd.config.enforceEntryGroup=false
</strong></code></pre>
