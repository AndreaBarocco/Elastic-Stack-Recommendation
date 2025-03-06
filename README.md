# Purpose
This repository provides a collection of optimizations and best practices to improve performance and security when using Elasticsearch, Kibana, and Logstash. The goal is to offer practical tips to optimize the configuration and usage of the Elastic Stack.

# Elasticsearch Configuration Recommendation
- Add Elastic Platinum License to the cluster to get support and use the x-pack features
- Avoid using jvm.options and use jvm.options.d, using jvm.options.d makes JVM management safer, more modular, and upgrade-proof, reducing the risk of errors and simplifying the configuration of the Elasticsearch cluster.
- Avoid defining -Xms and -Xmx settings and let Elasticsearch take care of it. To avoid that Elasticsearch works with uncompressed pointers, which can be detrimental for the performances
- Bootstrap memory lock Disable swapping | Elasticsearch Guide [8.X] | Elastic Configuring
system settings | Elasticsearch Guide [8.X] | Elastic
- cluster.initial_master_nodes setting should be removed once the cluster is up and running
- Use different Logical Volumes for data and logs

# Kibana Configuration Recommendation
- Set server.publicBaseUrl: setting used to generate links inside of Kibana
- Generate the Kibana Encryption keys with the tool kibana-encryption-keys:
  - xpack.encryptedSavedObjects.encryptionKey used to encrypt stored objects such as dashboards and visualizations (and notably API Keys inside Alerts definitions)
  - xpack.security.encryptionKey: used to encrypt session information
  - xpack.reporting.encryptionKey: used to encrypt saved reports
  - Afterward, store the encryption keys in the Kibana Keystore. It is recommended to store these keys in a separate password manager for DR purposes.
- security.cookieName: Sets the name of the cookie used for the session
- Deactivated [openssl-legacy-providers](https://www.elastic.co/guide/en/kibana/8.11/production.html#openssl-legacy-provider)
- Enable the sandbox feature: xpack.screenshotting.browser.chromium.disableSandbox

# Logstash Configuration Recommendation
- In the pipeline conf don't use username with clear-text password. Instead create a dedicated Logstash User by adapting the permissions configuring Logstash to use basic authentication, with a password stored in the Logstash Keystore
- jvm.options: the recommended heap size for typical ingestion scenarios should be no less than 4GB and no more than 8GB
