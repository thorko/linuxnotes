# helm

## create chart

```bash
helm create myname
```



## using environments

### values.yaml

```yaml
keepLogDays:
  _default: 2
  dev: 2
  prod: 5

esHost:
  _default: "elasticsearch.logging.svc"
  dev: "elasticsearch.logging.svc"
  prod: "elasticsearch.logging.svc"

curatorSchedule:
  _default: "10 9 * * *"
  dev: "10 9 * * *"
  prod: "15 20 * * *"

curatorVersion:
  _default: "praseodym/elasticsearch-curator:latest"
  dev: "praseodym/elasticsearch-curator:latest"
  prod: "praseodym/elasticsearch-curator:latest"
```

```yaml
unit_count: {{ pluck .Values.global.env .Values.keepLogDays | first | default .Values.keepLogDays._default }}
```

```yaml
{{- if eq .Values.global.env "prod" -}}
{{- end -}}
```



## install or upgrade

```bash
helm3 upgrade --install -n logging --set "global.env=dev" logging .
```



## get secrets and encode base64

_helpers.yaml

```yaml
{{- define "imagePullSecret" }}
{{- printf "{\"auths\": {\"%s\": {\"auth\": \"%s\"}}}" .Values.imageCredentials.registry (printf "%s:%s" .Values.imageCredentials.username .Values.imageCredentials.password | b64enc) | b64enc }}
{{- end }}
```

in another template call this template

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: myregistrykey
type: kubernetes.io/dockerconfigjson
data:
  .dockerconfigjson: {{ template "imagePullSecret" . }}
```



## set variable if/else

```yaml
// if global.loc == ams1 then es-master-1 else es-master-3
{{- $a := ternary "es-master-1" "es-master-3" (eq .Values.global.loc "ams1") }}
```



## get variables per env

```yaml
// list
elasticVersion:
  _default: 1
  prod: 2
  dev: 3
  
// get first element of list elasticVersion
{{ pluck .Values.global.env .Values.elasticVersion | first | default .Values.elasticVersion._default }}
```



## set variables

```yaml
// list
fluentdLimits:
  _default:
    memory: "256Mi"
  dev:
    memory: "256Mi"
  prod:
    memory: "512Mi"

{{- $fluentdLimits := pluck .Values.global.env .Values.fluentdLimits | first }}
{{ get $fluentdLimits "memory" }}
```

