[0KRunning with gitlab-runner 16.2.2 (30ac8e7f)[0;m
[0K  on prodgitci35lsat o2f6TXgJ, system ID: s_751bbc54ebd1[0;m
section_start:1704896601:resolve_secrets
[0K[0K[36;1mResolving secrets[0;m[0;m
section_end:1704896601:resolve_secrets
[0Ksection_start:1704896601:prepare_executor
[0K[0K[36;1mPreparing the "docker" executor[0;m[0;m
[0KUsing Docker executor with image docker.repo.usaa.com/usaa/grp-pi-helm/helm-deploy:3.2.0 ...[0;m
[0KUsing locally found image version due to "if-not-present" pull policy[0;m
[0KUsing docker image sha256:51e467d70fbe0718d8ac9c7d000fc45f0892831d9c84f68dd2367363ae20609b for docker.repo.usaa.com/usaa/grp-pi-helm/helm-deploy:3.2.0 with digest docker.repo.usaa.com/usaa/grp-pi-helm/helm-deploy@sha256:30bcd43b0a2aeadd4ea07b7abf0a64271a00efd2c6b96de62d45657cc74fa8ed ...[0;m
section_end:1704896603:prepare_executor
[0Ksection_start:1704896603:prepare_script
[0K[0K[36;1mPreparing environment[0;m[0;m
Updating CA certificates...
WARNING: ca-cert-ca.pem does not contain exactly one certificate or CRL: skipping
Running on prodgitci35lsat via prodgitci35lsat...
section_end:1704896604:prepare_script
[0Ksection_start:1704896604:get_sources
[0K[0K[36;1mGetting source from Git repository[0;m[0;m
[32;1mFetching changes with git depth set to 50...[0;m
Initialized empty Git repository in /builds/grp-ect-content-authoring/v1-metadata-services/.git/
[32;1mCreated fresh repository.[0;m
[32;1mChecking out 0147a756 as detached HEAD (ref is 57110_NewBranch)...[0;m

[32;1mSkipping Git submodules setup[0;m
section_end:1704896605:get_sources
[0Ksection_start:1704896605:download_artifacts
[0K[0K[36;1mDownloading artifacts[0;m[0;m
[32;1mDownloading artifacts for publish:chart:helm (164430753)...[0;m
Downloading artifacts from coordinator... ok      [0;m  host[0;m=prodgitlab.usaa.com id[0;m=164430753 responseStatus[0;m=200 OK token[0;m=64_gbqJt
section_end:1704896606:download_artifacts
[0Ksection_start:1704896606:step_script
[0K[0K[36;1mExecuting "step_script" stage of the job script[0;m[0;m
[0KUsing docker image sha256:51e467d70fbe0718d8ac9c7d000fc45f0892831d9c84f68dd2367363ae20609b for docker.repo.usaa.com/usaa/grp-pi-helm/helm-deploy:3.2.0 with digest docker.repo.usaa.com/usaa/grp-pi-helm/helm-deploy@sha256:30bcd43b0a2aeadd4ea07b7abf0a64271a00efd2c6b96de62d45657cc74fa8ed ...[0;m
[32;1m$ if [ -e "/pipeline_tools/scripts/gitlabci-pre-build.sh" ]; then source /pipeline_tools/scripts/gitlabci-pre-build.sh || true; fi && if [ -e "/usaa/dep-warn.sh" ]; then source /usaa/dep-warn.sh; fi && if [ -e "/usaa/pre-build.sh" ]; then source /usaa/pre-build.sh; fi && if [ -e "/pipeline_tools/scripts/datadog_pre_build_script.sh" ]; then source /pipeline_tools/scripts/datadog_pre_build_script.sh || true; fi[0;m
Sending Gitlab-CI custom pipeline tags to Datadog
More information can be found at http://go/tv2
Metrics sent
Tags sent
Tags sent
[32;1m$ if [ -z "$CI_COMMIT_TAG" ]; then # collapsed multi-line command[0;m
[32;1m$ rptok $HELM_LOCATION/values/$HELM_ENV.yaml[0;m
[32;1m$ helm-deploy $OCP_CLUSTER $OCP_PROJECT_NAME $OCP_APP_NAME $HELM_ENV $HELM_LOCATION[0;m
[32m[1mCluster is accepting deploys[0m
Checking kill switch status for https://api.tocp4sat08.usaa.com Proceeding with status: Working as intended

[36m==================== Logging in to the cluster ====================[0m

Confidant Version not runner instance
Attempt to fetch token directly from Confidant using https://intapi-inf.usaa.com/v2/confidant/access-token/gitlab-groups/grp-ect-content-authoring/projects/88354/pipelines/13056987/tokens for cluster https://api.tocp4sat08.usaa.com:6443...
Status code from Confidant: 200
Received valid response from Confidant. Attempting to parse token...
[34m[1moc login https://api.tocp4sat08.usaa.com:6443[0m
Logged into "https://api.tocp4sat08.usaa.com:6443" as "Jkp49ptWjQ" using the token provided.

You have access to 534 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".
Welcome! See 'oc help' to get started.
[34m[1moc project grp-ect-content-authoring[0m
Now using project "grp-ect-content-authoring" on server "https://api.tocp4sat08.usaa.com:6443".

[36m==================== Building and releasing v1-review-57110-newbranch-88354 ====================[0m

[34m[1mUsing Repo: helm-oci-test.repo.usaa.com[0m
[0Ksection_start:1704896620:helm_repo_login[collapsed=true]
[0K==== Helm Repo Login ====
Cyberark Login
2024/01/10 14:23:40 Attempting to retrieve Confidant credential for alias 'ca-alias-grp-ect-content-authoring'...
2024/01/10 14:23:40 Contacting Confidant V2...
2024/01/10 14:23:40 Request credentials from https://intapi-inf.usaa.com/v2/confidant/service-account/group-service-account-credentials?gitLabGroup=grp-ect-content-authoring&gitLabProjectID=88354&pipelineID=13056987&prefix=ca-alias...
2024/01/10 14:23:40 Confidant credential for alias 'ca-alias-grp-ect-content-authoring' succesfully retrieved.
Successfully fetched credentials via cyberark-tools
Login to Artifactory helm-oci-test.repo.usaa.com
Login Succeeded
[0Ksection_end:1704896621:helm_repo_login
[0K
[0Ksection_start:1704896621:helm_template_section[collapsed=true]
[0K==== Helm Template ====
[34m[1mhelm template[0m
[34m[1m    v1-review-57110-newbranch-88354[0m
[34m[1m    oci://helm-oci-test.repo.usaa.com/usaa/grp-ect-content-authoring/v1-metadata-services --version 1.0.0-57110-newbranch+0147a756[0m
[34m[1m    -f helm/values/review.yaml[0m
[34m[1m    --set global.git.repo="v1-metadata-services"[0m
[34m[1m    --set global.git.ref="57110-newbranch"[0m
[34m[1m    --set global.git.sha="0147a756"[0m
[34m[1m    --set global.gitlab.projectId="88354"[0m
[34m[1m    --set global.gitlab.pipeline="https://prodgitlab.usaa.com/grp-ect-content-authoring/v1-metadata-services/-/pipelines/13056987"[0m
[34m[1m    --set global.gitlab.environment="development-57110-5t969v"[0m
[34m[1m    --set global.ocp.cluster="https://api.tocp4sat08.usaa.com:6443"[0m
[34m[1m    --set global.ocp.portfolio="ent"[0m
[34m[1m    --set app_name="v1-review-57110-newbranch-88354"[0m
[34m[1m    --set cluster="https://api.tocp4sat08.usaa.com:6443"[0m
[34m[1m    --set namespace="grp-ect-content-authoring"[0m
[34m[1m     [0m
Pulled: helm-oci-test.repo.usaa.com/usaa/grp-ect-content-authoring/v1-metadata-services:1.0.0-57110-newbranch_0147a756
Digest: sha256:c5daa14377f0c50e901af647fb6725e5aaaf31d1012acc59de1d02da94f1bcc1
helm-oci-test.repo.usaa.com/usaa/grp-ect-content-authoring/v1-metadata-services:1.0.0-57110-newbranch_0147a756 contains an underscore.

OCI artifact references (e.g. tags) do not support the plus sign (+). To support
storing semantic versions, Helm adopts the convention of changing plus (+) to
an underscore (_) in chart version tags when pushing to a registry and back to
a plus (+) when pulling from a registry.
---
# Source: v1-metadata-services/charts/usaa-talon-api/templates/envoy/EnvoyConfigMap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: v1-review-57110-newbranch-88354-envoy
  labels:
    
    helm.sh/chart: usaa-talon-api-0.21.1
    app.kubernetes.io/name: usaa-talon-api
    app.kubernetes.io/instance: v1-review-57110-newbranch-88354
    app: v1-review-57110-newbranch-88354
    usaa_env: review
    app.kubernetes.io/managed-by: Helm
  annotations:
    
    pi.usaa.com/api-framework: "spring"
data:
  envoy.yaml: |-
    ## Layered Configuration
    layered_runtime:
      layers:
      - name: static_layer_0
        static_layer:
          # Use the Overload Manager to globally limit connections
          # Envoy Best Practices: https://www.envoyproxy.io/docs/envoy/latest/configuration/best_practices/edge#best-practices-edge
          overload:
            global_downstream_max_connections: 50000
    ## Envoy Node Information
    node:
      id: "v1-review-57110-newbranch-88354"
      cluster: "v1-review-57110-newbranch-88354"
    ## Admin Console Configuration
    admin:
      access_log:
      - name: envoy.access_loggers.file
        typed_config:
          "@type": "type.googleapis.com/envoy.extensions.access_loggers.file.v3.FileAccessLog"
          path: /tmp/admin_access.log
      address:
        socket_address: { address: 0.0.0.0, port_value: 9901 }
    ## Static Resource Configuration
    static_resources:
      listeners:
      - name: listener_0
        address:
          socket_address: { address: 0.0.0.0, port_value: 10000 }
        filter_chains:
        - filters:
          - name: envoy.http_connection_manager
            typed_config:
              "@type": type.googleapis.com/envoy.extensions.filters.network.http_connection_manager.v3.HttpConnectionManager
              access_log:
              # Configure Listener to access log with JSON formatter
              - name: envoy.access_loggers.file
                typed_config:
                  "@type": "type.googleapis.com/envoy.extensions.access_loggers.file.v3.FileAccessLog"
                  path: /dev/stdout
                  log_format:
                    json_format:
                      "@timestamp": "%START_TIME%"
                      "duration": "%DURATION%"
                      "bytes_received": "%BYTES_RECEIVED%"
                      "bytes_sent": "%BYTES_SENT%"
                      "authority": "%REQ(:AUTHORITY)%"
                      "method": "%REQ(:METHOD)%"
                      "path": "%REQ(:PATH)%"
                      "response_code": "%RESPONSE_CODE%"
                      "response_flags": "%RESPONSE_FLAGS%"
                      "cluster": "%UPSTREAM_CLUSTER%"
              stat_prefix: ingress_http
              codec_type: AUTO
              tracing:
                random_sampling:
                  value: "1"
                provider:
                  name: envoy.tracers.zipkin
                  typed_config:
                    "@type": type.googleapis.com/envoy.config.trace.v3.ZipkinConfig
                    collector_cluster: jaeger
                    collector_endpoint: "/api/v2/spans"
                    collector_endpoint_version: HTTP_JSON
                    shared_span_context: false
              route_config:
                name: local_route
                virtual_hosts:
                - name: local_service
                  domains: ["*"]
                  ##
                  ## User provided routes
                  ##
                  routes:
                  - match:
                      prefix: /v1/enterprise/metadata-services/actuator/
                    route:
                      cluster: 'v1-review-57110-newbranch-88354'
                      host_rewrite_literal: localhost
                    typed_per_filter_config:
                      envoy.filters.http.ext_authz:
                        '@type': type.googleapis.com/envoy.extensions.filters.http.ext_authz.v3.ExtAuthzPerRoute
                        disabled: true
                  - match:
                      prefix: /
                    route:
                      cluster: 'v1-review-57110-newbranch-88354'
                      host_rewrite_literal: localhost
                  ## End user provided routes
              http_filters:
              - name: envoy.filters.http.ext_authz
                typed_config:
                  "@type": type.googleapis.com/envoy.extensions.filters.http.ext_authz.v3.ExtAuthz
                  transport_api_version: V3
                  grpc_service:
                    timeout: "1s"
                    envoy_grpc:
                      cluster_name: ext-authz
              - name: envoy.filters.http.rbac
                typed_config:
                  "@type": type.googleapis.com/envoy.extensions.filters.http.rbac.v3.RBAC
                  ##
                  ## User provided RBAC
                  ##
                  rules:
                    action: ALLOW
                    policies:
                      AUTH_POLICY:
                        permissions:
                        - any: true
                        principals:
                        - header:
                            contains_match: ROLE_GENESIS_EXP
                            name: roles
                      HEALTH_POLICY:
                        permissions:
                        - header:
                            name: :path
                            prefix_match: /v1/enterprise/metadata-services/actuator
                        principals:
                        - any: true
                  ## End user provided RBAC
              - name: envoy.filters.http.router
                typed_config:
                  "@type": type.googleapis.com/envoy.extensions.filters.http.router.v3.Router
                  start_child_span: true
      clusters:
      ## This section is routing for the External Authz service.
      - name: ext-authz
        type: STRICT_DNS
        connect_timeout: 1s
        typed_extension_protocol_options:
          envoy.extensions.upstreams.http.v3.HttpProtocolOptions:
            "@type": type.googleapis.com/envoy.extensions.upstreams.http.v3.HttpProtocolOptions
            # Enable HTTP/2 for gRPC
            explicit_http_config:
              http2_protocol_options: {}
        load_assignment:
          cluster_name: ext-authz
          endpoints:
          - lb_endpoints:
            - endpoint:
                address:
                  socket_address:
                    address: 127.0.0.1
                    port_value: 9000
      ## This section is routing for your application.
      - name: "v1-review-57110-newbranch-88354"
        connect_timeout: 1s
        type: STRICT_DNS
        dns_lookup_family: V4_ONLY
        lb_policy: ROUND_ROBIN
        load_assignment:
          cluster_name: "v1-review-57110-newbranch-88354"
          endpoints:
          - lb_endpoints:
            - endpoint:
                address:
                  socket_address:
                    address: 127.0.0.1
                    port_value: 8080
      ## This section is routing for the jaeger service.
      - name: jaeger
        connect_timeout: 1s
        type: STRICT_DNS
        lb_policy: ROUND_ROBIN
        load_assignment:
          cluster_name: jaeger
          endpoints:
          - lb_endpoints:
            - endpoint:
                address:
                  socket_address:
                    address: "jaeger-collector.jaeger.svc.cluster.local"
                    port_value: 9411
---
# Source: v1-metadata-services/charts/usaa-talon-api/templates/extauth/RolesConfigMap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: v1-review-57110-newbranch-88354-rbac
  labels:
    
    helm.sh/chart: usaa-talon-api-0.21.1
    app.kubernetes.io/name: usaa-talon-api
    app.kubernetes.io/instance: v1-review-57110-newbranch-88354
    app: v1-review-57110-newbranch-88354
    usaa_env: review
    app.kubernetes.io/managed-by: Helm
  annotations:
    
    pi.usaa.com/api-framework: "spring"
data:
  role-mapping-dev.yaml: |-
    ROLE_GENESIS_EXP:
    - ASG_CONTENT_AUTHORING_LIBRARIAN_EXP_TEST
  role-mapping-test.yaml: |-
    ROLE_GENESIS_EXP:
    - ASG_CONTENT_AUTHORING_LIBRARIAN_EXP_TEST
  role-mapping-prod.yaml: |-
    ROLE_GENESIS_EXP:
    - ASG_CONTENT_AUTHORING_LIBRARIAN_EXP_PROD
---
# Source: v1-metadata-services/charts/usaa-talon-api/templates/Service.yaml
apiVersion: v1
kind: Service
metadata:
  name: v1-review-57110-newbranch-88354
  labels:
    
    helm.sh/chart: usaa-talon-api-0.21.1
    app.kubernetes.io/name: usaa-talon-api
    app.kubernetes.io/instance: v1-review-57110-newbranch-88354
    app: v1-review-57110-newbranch-88354
    usaa_env: review
    app.kubernetes.io/managed-by: Helm
  annotations:
    
    pi.usaa.com/api-framework: "spring"
spec:
  type: ClusterIP
  ports: 
  - name: http
    port: 8080
    protocol: TCP
    targetPort: http
  selector:
    app.kubernetes.io/name: usaa-talon-api
    app.kubernetes.io/instance: v1-review-57110-newbranch-88354
    app: v1-review-57110-newbranch-88354
    usaa_env: review
  sessionAffinity: None
---
# Source: v1-metadata-services/charts/usaa-talon-api/templates/Deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: v1-review-57110-newbranch-88354
  labels:
    deployment: v1-review-57110-newbranch-88354
    
    helm.sh/chart: usaa-talon-api-0.21.1
    app.kubernetes.io/name: usaa-talon-api
    app.kubernetes.io/instance: v1-review-57110-newbranch-88354
    app: v1-review-57110-newbranch-88354
    usaa_env: review
    app.kubernetes.io/managed-by: Helm
  annotations:
    
    pi.usaa.com/api-framework: "spring"
    usaa.ltm.route: v1-review-57110-newbranch-88354

spec:
  revisionHistoryLimit: 2
  replicas: 1
  progressDeadlineSeconds: 660
  selector:
    matchLabels:
      deployment: v1-review-57110-newbranch-88354
      app.kubernetes.io/name: usaa-talon-api
      app.kubernetes.io/instance: v1-review-57110-newbranch-88354
      app: v1-review-57110-newbranch-88354
      usaa_env: review
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        deployment: v1-review-57110-newbranch-88354
        
        helm.sh/chart: usaa-talon-api-0.21.1
        app.kubernetes.io/name: usaa-talon-api
        app.kubernetes.io/instance: v1-review-57110-newbranch-88354
        app: v1-review-57110-newbranch-88354
        usaa_env: review
        app.kubernetes.io/managed-by: Helm
      annotations:
        
        pi.usaa.com/api-framework: "spring"
        
        usaa.conjur.sidecar: "sidecar"
        
        prometheus.io/scrape: "true"
        prometheus.io/port: "8080"
        prometheus.io/path: /v1/enterprise/metadata-services/actuator/prometheus
    spec:
      topologySpreadConstraints:
      - maxSkew: 1
        topologyKey: kubernetes.io/hostname
        whenUnsatisfiable: ScheduleAnyway
        labelSelector:
          matchLabels:
            deployment: v1-review-57110-newbranch-88354
      automountServiceAccountToken: false
      containers:
      
      - image: "docker-test.repo.usaa.com/usaa/grp-ect-content-authoring/v1-metadata-services:0147a756"
        imagePullPolicy: Always
        name: v1-review-57110-newbranch-88354
        env:
        - name: RUNTIME_ENV
          value: review
        - name: NAMESPACE
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
        - name: POD
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.name
        - name: OPENTRACING_JAEGER_UDP_SENDER_HOST
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: status.hostIP
        - name: OPENTRACING_JAEGER_UDP_SENDER_PORT
          value: "6831"
        - name: VAULT_TYPE
          value: CONJUR
        envFrom:
        - configMapRef:
            name: environment-config-map
        livenessProbe:
          
          exec:
            command:
            - curl
            - --fail
            - http://localhost:8080/v1/enterprise/metadata-services/actuator/health/liveness
          failureThreshold: 5
          initialDelaySeconds: 75
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 5
        readinessProbe:
          
          exec:
            command:
            - curl
            - --fail
            - http://localhost:8080/v1/enterprise/metadata-services/actuator/health/readiness
          failureThreshold: 5
          initialDelaySeconds: 75
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 5
        startupProbe:
          
          exec:
            command:
            - curl
            - --fail
            - http://localhost:8080/v1/enterprise/metadata-services/actuator/health/liveness
          failureThreshold: 5
          initialDelaySeconds: 75
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 5
        resources:
          limits:
            cpu: 500m
            memory: 1536Mi
          requests:
            cpu: 25m
            memory: 1536Mi
        terminationMessagePath: /dev/termination-log
        volumeMounts:
        - mountPath: /etc/pki/ca-trust/extracted/pem
          name: usaa-trusted-ca
          readOnly: true
      - name: envoy-proxy
        image: "docker.repo.usaa.com/envoyproxy/envoy:v1.26.3"
        ports:
        - name: http
          containerPort: 10000
          protocol: TCP
        command:
        - /usr/local/bin/envoy
        args:
        - -c
        - /config/envoy.yaml
        resources:
          limits:
            cpu: 100m
            memory: 256Mi
          requests:
            cpu: 50m
            memory: 128Mi
        livenessProbe:
          tcpSocket:
            port: 10000
        readinessProbe:
          httpGet:
            path: /ready
            port: 9901
            scheme: HTTP
        volumeMounts:
        - name: envoy-config
          mountPath: /config
      - name: extauth
        image: "docker-prod.repo.usaa.com/usaa/grp-external-authz/external-authz-svc:v4.3.1"
        imagePullPolicy: Always
        ports:
        - name: grpc-authz
          containerPort: 9000
          protocol: TCP
        env:
        - name: AUTHZ_ROLE_MAPPING_CONFIG_DIR
          value: /config
        - name: PORTFOLIO
          value: ENT
        envFrom:
        - configMapRef:
            name: environment-config-map
        startupProbe:
          tcpSocket:
            port: 9000
          # We have a generous startupProbe to allow
          # time for large groups to be cached from Directory
          initialDelaySeconds: 5
          failureThreshold: 12
          successThreshold: 1
          periodSeconds: 10
        readinessProbe:
          tcpSocket:
            port: 9000
          initialDelaySeconds: 5
          periodSeconds: 10
        livenessProbe:
          tcpSocket:
            port: 9000
          initialDelaySeconds: 5
          periodSeconds: 10
        resources:
          limits:
            cpu: 100m
            memory: 256Mi
          requests:
            cpu: 50m
            memory: 128Mi
        volumeMounts:
        - name: rbac-config # Mount the config volume that has the role mappings
          mountPath: /config
      volumes:
      - name: envoy-config
        configMap:
          name: "v1-review-57110-newbranch-88354-envoy"
      - name: rbac-config
        configMap:
          name: "v1-review-57110-newbranch-88354-rbac"
      - name: usaa-trusted-ca
        configMap:
          name: "usaa-ca-certs"
          items:
            - key: ca-bundle.crt
              path: tls-ca-bundle.pem
      dnsPolicy: ClusterFirst
      dnsConfig:
        searches:
          - usaa.com
          - eagle.usaa.com
      restartPolicy: Always
      terminationGracePeriodSeconds: 60
---
# Source: v1-metadata-services/charts/usaa-talon-api/charts/usaa-gloo-api/templates/AuthConfig.yaml
apiVersion: enterprise.gloo.solo.io/v1
kind: AuthConfig
metadata:
  name: v1-review-57110-newbranch-88354
  labels:    
    helm.sh/chart: usaa-gloo-api-0.6.1
    app.kubernetes.io/name: usaa-gloo-api
    app.kubernetes.io/instance: v1-review-57110-newbranch-88354
    app: v1-review-57110-newbranch-88354
    usaa_env: review
    app.kubernetes.io/managed-by: Helm    
    apiId: a56ef025-8916-4edb-907c-6cbdd9c755b0
    domain: rintapi-ent.usaa.com
  annotations:    
    pi.usaa.com/api-framework: "spring"
spec:
  configs:
  - name: ApiKeyAuth
    apiKeyAuth:
      headerName: api-key
      labelSelector:
        apiId: a56ef025-8916-4edb-907c-6cbdd9c755b0
---
# Source: v1-metadata-services/charts/usaa-talon-api/charts/usaa-gloo-api/templates/RouteTable.yaml
apiVersion: gateway.solo.io/v1
kind: RouteTable
metadata:
  name: v1-review-57110-newbranch-88354
  labels:    
    helm.sh/chart: usaa-gloo-api-0.6.1
    app.kubernetes.io/name: usaa-gloo-api
    app.kubernetes.io/instance: v1-review-57110-newbranch-88354
    app: v1-review-57110-newbranch-88354
    usaa_env: review
    app.kubernetes.io/managed-by: Helm    
    apiId: a56ef025-8916-4edb-907c-6cbdd9c755b0
    domain: rintapi-ent.usaa.com
  annotations:
    pi.usaa.com/api-framework: "spring"
spec:
  weight: 40
  routes:
  - matchers: # stable
    - prefix: /v1/enterprise/metadata-services
      methods:
        - GET      
      headers:
        - name: Cookie
          regex: true
          value: ".*glooenv88354=v1-review-57110-newbranch-88354.*"    
    options:    
      extauth:
        configRef:
          name: v1-review-57110-newbranch-88354
          namespace: grp-ect-content-authoring  
      timeout: 5s
    routeAction:
      single:
        upstream:
          name: v1-review-57110-newbranch-88354
          namespace: grp-ect-content-authoring
---
# Source: v1-metadata-services/charts/usaa-talon-api/templates/ServiceMonitor.yaml
kind: ServiceMonitor
apiVersion: monitoring.coreos.com/v1
metadata:
  name: v1-review-57110-newbranch-88354
  labels:
    
    helm.sh/chart: usaa-talon-api-0.21.1
    app.kubernetes.io/name: usaa-talon-api
    app.kubernetes.io/instance: v1-review-57110-newbranch-88354
    app: v1-review-57110-newbranch-88354
    usaa_env: review
    app.kubernetes.io/managed-by: Helm
  annotations:
    
    pi.usaa.com/api-framework: "spring"
spec:
  jobLabel: v1-review-57110-newbranch-88354
  selector:
    matchLabels:
      app.kubernetes.io/name: usaa-talon-api
      app.kubernetes.io/instance: v1-review-57110-newbranch-88354
      app: v1-review-57110-newbranch-88354
      usaa_env: review
  namespaceSelector:
    matchNames:
    - grp-ect-content-authoring
  endpoints:
    - interval: 30s
      path: /v1/enterprise/metadata-services/actuator/prometheus
      port: http
---
# Source: v1-metadata-services/charts/usaa-talon-api/charts/usaa-gloo-api/templates/Upstream.yaml
apiVersion: gloo.solo.io/v1
kind: Upstream
metadata:
  name: v1-review-57110-newbranch-88354
  labels:    
    helm.sh/chart: usaa-gloo-api-0.6.1
    app.kubernetes.io/name: usaa-gloo-api
    app.kubernetes.io/instance: v1-review-57110-newbranch-88354
    app: v1-review-57110-newbranch-88354
    usaa_env: review
    app.kubernetes.io/managed-by: Helm    
    apiId: a56ef025-8916-4edb-907c-6cbdd9c755b0
    domain: rintapi-ent.usaa.com
  annotations:    
    pi.usaa.com/api-framework: "spring"
spec:
  kube:
    serviceName: v1-review-57110-newbranch-88354
    serviceNamespace: grp-ect-content-authoring
    servicePort: 8080
  useHttp2: false
[0Ksection_end:1704896621:helm_template_section
[0K
==== Helm Upgrade ====
[34m[1mhelm upgrade[0m
[34m[1m    v1-review-57110-newbranch-88354[0m
[34m[1m    oci://helm-oci-test.repo.usaa.com/usaa/grp-ect-content-authoring/v1-metadata-services --version 1.0.0-57110-newbranch+0147a756[0m
[34m[1m    -f helm/values/review.yaml[0m
[34m[1m    --install[0m
[34m[1m    --atomic[0m
[34m[1m    --render-subchart-notes[0m
[34m[1m    --wait[0m
[34m[1m    --timeout 5m[0m
[34m[1m    --set global.git.repo="v1-metadata-services"[0m
[34m[1m    --set global.git.ref="57110-newbranch"[0m
[34m[1m    --set global.git.sha="0147a756"[0m
[34m[1m    --set global.gitlab.projectId="88354"[0m
[34m[1m    --set global.gitlab.pipeline="https://prodgitlab.usaa.com/grp-ect-content-authoring/v1-metadata-services/-/pipelines/13056987"[0m
[34m[1m    --set global.gitlab.environment="development-57110-5t969v"[0m
[34m[1m    --set global.ocp.cluster="https://api.tocp4sat08.usaa.com:6443"[0m
[34m[1m    --set global.ocp.portfolio="ent"[0m
[34m[1m    --set app_name="v1-review-57110-newbranch-88354"[0m
[34m[1m    --set cluster="https://api.tocp4sat08.usaa.com:6443"[0m
[34m[1m    --set namespace="grp-ect-content-authoring"[0m
[34m[1m     [0m
[34m[1m    [0m
Release "v1-review-57110-newbranch-88354" does not exist. Installing it now.
Pulled: helm-oci-test.repo.usaa.com/usaa/grp-ect-content-authoring/v1-metadata-services:1.0.0-57110-newbranch_0147a756
Digest: sha256:c5daa14377f0c50e901af647fb6725e5aaaf31d1012acc59de1d02da94f1bcc1
helm-oci-test.repo.usaa.com/usaa/grp-ect-content-authoring/v1-metadata-services:1.0.0-57110-newbranch_0147a756 contains an underscore.

OCI artifact references (e.g. tags) do not support the plus sign (+). To support
storing semantic versions, Helm adopts the convention of changing plus (+) to
an underscore (_) in chart version tags when pushing to a registry and back to
a plus (+) when pulling from a registry.
Error: release v1-review-57110-newbranch-88354 failed, and has been uninstalled due to atomic being set: context deadline exceeded
[31m[1mA failure occurred. Exiting script...[0m

[36m==================== Helpful Links ====================[0m

 _____ ____   ___  _   _ ____  _     _____ ____  _   _  ___   ___ _____ ___ _   _  ____   
|_   _|  _ \ / _ \| | | | __ )| |   | ____/ ___|| | | |/ _ \ / _ \_   _|_ _| \ | |/ ___|_ 
  | | | |_) | | | | | | |  _ \| |   |  _| \___ \| |_| | | | | | | || |  | ||  \| | |  _(_)
  | | |  _ <| |_| | |_| | |_) | |___| |___ ___) |  _  | |_| | |_| || |  | || |\  | |_| |_ 
  |_| |_| \_\\___/ \___/|____/|_____|_____|____/|_| |_|\___/ \___/ |_| |___|_| \_|\____(_)
- Check [36m[1mhttp://go/helmissues[0m for common issues during Helm deployment
- Check [36m[1mhttp://go/gloovalidatordocs[0m for help with Gloo validation issues
[32m[1m- You can check the OpenShift web-console via this link:[0m
    -> [36m[1mhttps://console-openshift-console.apps.tocp4sat08.usaa.com/k8s/cluster/projects/grp-ect-content-authoring[0m
[32m[1m- You can check Kibana event logs via this link:[0m
    -> Test kibana logs: [36m[1mhttp://go/tocpkibanaevents/tocp4sat08/grp-ect-content-authoring/v1-review-57110-newbranch-88354[0m
[32m[1m- If a container was created you can view container logs via this link:[0m
    -> Test container logs: [36m[1mhttp://go/tdeploylogs/v1-review-57110-newbranch-88354[0m
[32m[1m- You can check Gloo access logs via this link:[0m
    -> Test Gloo access logs: [36m[1mhttp://go/tgloologs/v1-review-57110-newbranch-88354/grp-ect-content-authoring[0m
 _____________
|             |
|   LOGS!     |
|_____________|
 /\_/\ ||
( *^* )||
/     >'|
section_end:1704896925:step_script
[0Ksection_start:1704896925:cleanup_file_variables
[0K[0K[36;1mCleaning up project directory and file based variables[0;m[0;m
section_end:1704896925:cleanup_file_variables
[0K[31;1mERROR: Job failed: exit code 1
[0;m
