---
title: "Disallow Localhost ExternalName Services"
category: Sample
version: 
subject: Service
policyType: "validate"
description: >
    A Service of type ExternalName which points back to localhost can potentially be used to exploit vulnerabilities in some Ingress controllers. This policy audits Services of type ExternalName if the externalName field refers to localhost.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/disallow_localhost_services/disallow_localhost_services.yaml" target="-blank">/other/disallow_localhost_services/disallow_localhost_services.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: no-localhost-service
  annotations:
    policies.kyverno.io/title: Disallow Localhost ExternalName Services
    policies.kyverno.io/category: Sample
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Service
    policies.kyverno.io/description: >-
      A Service of type ExternalName which points back to localhost can potentially be used to exploit
      vulnerabilities in some Ingress controllers. This policy audits Services of type ExternalName
      if the externalName field refers to localhost.
spec:
  validationFailureAction: audit
  background: true
  rules:
  - name: no-localhost-service
    match:
      resources:
        kinds:
        - Service
    validate:
      message: "Service of type ExternalName cannot point to localhost."
      pattern:
        spec:
          (type): ExternalName
          externalName: "!localhost"
```
