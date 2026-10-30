ConfigMaps in Kubernetes are used to store non-confidential configuration data in key-value pairs, allowing you to decouple configuration from pod specifications and make your applications more portable. They can be consumed by pods as environment variables and updated without rebuilding your application container.

apiVersion: v1
kind: ConfigMap
metadata:
  name: game-config
data:
  MONGODB_HOST: "mongodb"
  MONGODB_PORT: "27017"
  Profile: "dev"


Secrets in Kubernetes are similar to ConfigMaps but are specifically designed for sensitive information like passwords, OAuth tokens, and SSH keys. They can be consumed by pods similarly to ConfigMaps. Secrets use the data field, which expects base64 encoded values. This encoding is particularly useful for handling special characters often found in sensitive data:

apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  MONGODB_USER: 'cm9vdA=='    # base64 encoded value of "root"
  MONGODB_PASSWORD: 'UGFzc3dvcmQxMjM='    # base64 encoded value of "Password123"


Important Note: while Secrets are base64 encoded, they are not encrypted. Additional security measures are typically implemented to protect sensitive data in clusters. These can include external secret management systems, or implementing Kubernetes Encryption Providers. However, specific approaches are beyond the scope of this overview as they vary based on organizational needs and security policies.