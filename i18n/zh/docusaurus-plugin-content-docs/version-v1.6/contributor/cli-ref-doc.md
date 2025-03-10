---
title: Update Reference Docs
---

This guide is an introduction of how to generate docs automatically from Golang scripts.

## CLI Reference

This section introduce how to update [CLI reference doc](../cli/vela) in the website.
The whole process was done by script automatically. You need to follow this guide to build your environment.

1. step up these two projects in the same folder.

```shell
$ tree -L 1
.
├── kubevela
└── kubevela.io
```

2. Run generate command in kubevela root dir.

```shell
cd kubevela/
go run ./hack/docgen/cli/gen.go
```

3. Update more docs such as i18n zh

```shell
go run ./hack/docgen/cli/gen.go ../kubevela.io/i18n/zh/docusaurus-plugin-content-docs/current/cli
```

4. Then you can check the difference in kubevela.io.

```shell
cd ../kubevela.io
git status
```

5. Check in the changes and send pull request.

## Built-in Definition Reference

This section introduce how to definition reference docs in the website, including:

- [component definition reference doc](../end-user/components/references).
- [trait definition reference doc](../end-user/traits/references).
- [policy definition reference doc](../end-user/policies/references).
- [workflow step definition reference doc](../end-user/workflow/built-in-workflow-defs).

Most of the steps are done by script automatically. You need to follow this guide to build your environment.

By default, the following steps will update for all definition reference at a time.
Just follow these steps.

1. step up these two projects in the same folder.

```shell
$ tree -L 1
.
├── kubevela
└── kubevela.io
```

2. Run generate command in kubevela root dir.

```shell
cd kubevela/
go run hack/docgen/def/gen.go
```

3. Then you can check the difference in kubevela.io.

```shell
cd ../kubevela.io
git status
```

4. Check in the changes and send pull request.

That's finished for the general update.

### Update for Specific 

You can specify some args for more flexible usage.

* Generate only for specified type

```shell
go run hack/docgen/def/gen.go --type component
go run hack/docgen/def/gen.go --type trait
go run hack/docgen/def/gen.go --type policy
go run hack/docgen/def/gen.go --type workflowstep
```

* Specify a path for output

> You must choose one of the type generated if path specified.

```shell
go run hack/docgen/def/gen.go --type component --path ../kubevela.io/docs/end-user/components/references.md
```

* Specify the i18n location with a translation script data for the output

```shell
go run hack/docgen/def/gen.go --location zh --i18n ../kubevela.io/static/reference-i18n.json

go run hack/docgen/def/gen.go --location zh --i18n https://kubevela.io/reference-i18n.json

go run hack/docgen/def/gen.go --type component --path ../kubevela.io/docs/end-user/components/references.md --location zh --i18n ../kubevela.io/static/reference-i18n.json
```

### How the docs generated?

1. Load definitions from Kubernetes.

The script will load all definitions from your Kubernetes Cluster, so you need to apply all built-in definitions before run.

2. Compare the internal definition folder in the project.

Besides the server side definitions, the script will also compare the cue files in the KubeVela project. Only definitions existing in these folders will be generated as reference docs.

```console
$ tree ./vela-templates/definitions/internal
vela-templates/definitions/internal
├── component
│   ├── config-image-registry.cue
│   ├── ...
│   └── worker.cue
├── policy
│   ├── health.cue
│   ├── override.cue
│   └── topology.cue
├── trait
│   ├── affinity.cue
│   ├── ...
│   └── storage.cue
└── workflowstep
    ├── apply-application-in-parallel.cue
    ├── ...
    └── webhook.cue

4 directories, 53 files
```

3. Generate Examples.

By default, the definition don't contain any examples, maintainers can specify example for the built-in definitions.

The docs folder will be embedded into CLI binary, you must write into the following hierarchy:

```console
$ tree references/plugins/def-doc
references/plugins/def-doc
├── component
│   ├── webservice.eg.md
│   ├── webservice.param.md
│   ├── webservice.desc.md
│   ├── ...
│   └── worker.eg.md
└── trait
    ├── annotations.eg.md
    ├── ...
    └── sidecar.eg.md

2 directories, 21 files
```

The file name **MUST** has the same name with the definition, along with the suffix:

* The example of definition **MUST** has suffix `.eg.md` and write in markdown format.
* The parameter(specification) of definition **MUST** has suffix `.param.md`, if exists, it will override the one auto-generated from definition parameters.
* The description of definition **MUST** has suffix `.desc.md`, if exists, it will override the one auto-generated from definition annotation.


## Terraform Based Cloud Resource Reference

This section introduce how to update [terraform based cloud resource reference doc](../end-user/components/cloud-services/cloud-resources-list) in the website.

Most of the steps are done by script automatically. You need to follow this guide to build your environment.

1. step up these two projects in the same folder.

```shell
$ tree -L 1
.
├── kubevela
└── kubevela.io
```

2. Run generate command in kubevela root dir.

```shell
cd kubevela/
go run ./hack/docgen/terraform/generate.go --i18n ../kubevela.io/static/reference-i18n.json
```

3. Update the resource catalog list if there're new cloud resources
  - Resource Catalog(English): `kubevela.io/docs/end-user/components/cloud-services/cloud-resources-list.md`
  - Resource Catalog(Chinese): `kubevela.io/i18n/zh/docusaurus-plugin-content-docs/current/end-user/components/cloud-services/cloud-resources-list.md`

4. Then you can check the difference in kubevela.io.

```shell
cd ../kubevela.io
git status
```

5. Check in the changes and send pull request.

## Translate for the reference docs

You need to translate into this file if you want to contribute for internationalization(i18n):

- File path: `kubevela.io/static/reference-i18n.json`
- URL for public access: https://kubevela.io/reference-i18n.json

After any translate, run the commands above to update all these reference docs.