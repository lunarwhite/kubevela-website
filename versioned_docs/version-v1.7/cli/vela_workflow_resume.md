---
title: vela workflow resume
---

Resume a suspend workflow.

### Synopsis

Resume a suspend workflow in cluster.

```
vela workflow resume [flags]
```

### Examples

```
vela workflow resume <workflow-name>
```

### Options

```
  -e, --env string         specify environment name for application
  -h, --help               help for resume
  -n, --namespace string   specify the Kubernetes namespace to use
  -t, --type string        the type of the resource, support: [app, workflow]
```

### Options inherited from parent commands

```
  -y, --yes   Assume yes for all user prompts
```

### SEE ALSO

* [vela workflow](vela_workflow)	 - Operate application delivery workflow.

#### Go Back to [CLI Commands](vela) Homepage.


###### Auto generated by [spf13/cobra script in KubeVela](https://github.com/kubevela/kubevela/tree/master/hack/docgen).
