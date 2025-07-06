# APISIX OpenShift Helm Chart

Wrapper Helm Chart for Apache APISIX on OpenShift

## Deployment

Execute the following command to deploy the chart to OpenShift to a namespace called `apisix`:

```shell
helm upgrade -i -n apisix --create-namespace apisix .  --values - <<EOF

apisix:
  ingress:
    hosts:
      - paths:
          - /
        host: apisix.apps.$(oc get dns cluster -o jsonpath='{ .spec.baseDomain }')
  dashboard:
    ingress:
      hosts:
        - paths:
            - /
          host: apisix-dashboard.apps.$(oc get dns cluster -o jsonpath='{ .spec.baseDomain }')
EOF
```

Two _Routes_ were created during the installation of the Chart:

* APISIX
* APISIX Dashboard

Use the following commands to locate the URL's for each of the services:

```shell
# APISIX URL
echo https://$(oc get ingress -n apisix apisix -o jsonpath='{ .spec.rules[0].host }')

# APISIX Dashboard URL
echo https://$(oc get ingress -n apisix apisix-dashboard -o jsonpath='{ .spec.rules[0].host }')
```