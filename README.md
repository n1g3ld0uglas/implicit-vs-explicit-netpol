# Implicit vs. Explicit Default/Deny Network Policies

If a condition in a network policy statement is not met, the request generally results in a default-deny (in most organizations). 
If all conditions in a statement are met, the request results in either an Allow or an Explicit-deny based on the value of the Effect element of the policy.
Network Policies don't specify what to do if a condition isn't met, so the default result in this case should be a default-deny.

# Implicit

An implicit deny is when a user or group are not granted a specific permission in the security settings of an object, but they are not explicitly denied either.
This default deny policy gives other policies a chance to apply, and if traffic is not matched, it will finally be denied.

```
apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
metadata:
  name: fortimanager.ns1-to-app1-implicit-deny
spec:
  tier: fortimanager
  order: 100
  selector: tigera.io/address-group == "app-1-address-group"
  namespaceSelector: ''
  serviceAccountSelector: ''
  doNotTrack: false
  applyOnForward: false
  preDNAT: false
  types:
    - Ingress
    - Egress
```

# Explicit

The explicit deny is when the administrator has selected the Deny option for a permission for a user or group. 
This Deny takes precedence over all allowed settings. The administrator has explicitly set the permission, and there is no way around it.

```
apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
metadata:
  name: fortimanager.ns1-to-app1-explicit-deny
spec:
  tier: fortimanager
  order: 300
  selector: tigera.io/address-group == "app-1-address-group"
  namespaceSelector: ''
  serviceAccountSelector: ''
  ingress:
    - action: Deny
      source: {}
      destination: {}
  egress:
    - action: Deny
      source: {}
      destination: {}
  doNotTrack: false
  applyOnForward: false
  preDNAT: false
  types:
    - Ingress
    - Egress
```

# Best Practices for Default Deny Policies


