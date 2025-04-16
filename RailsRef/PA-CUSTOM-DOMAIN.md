
# [Mapping custom domains](https://cloud.google.com/run/docs/mapping-custom-domains)

  - ## [Map custom domains using a global external Application Load Balancer](https://cloud.google.com/run/docs/integrate/custom-domain-load-balancer)
    - ## initial config
      https://console.cloud.google.com/run/detail/europe-west2/photo-app-0-svc/integrations?authuser=0&project=heidless-ror-2

    - ## DNS Config (123 Reg: heidless.com)
    ```
    TYPE:
    A
    NAME:
    heidless.com
    TTL:
    3600
    DATA:
    35.241.63.130
    
    ```

# DOMAIN MAPPING 
## only available in 10 geographies
```
asia-east1
asia-northeast1
asia-southeast1
europe-north1
europe-west1
europe-west4
us-central1
us-east1
us-east4
us-west1

```



