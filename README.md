# Simple S3 Resource for [Concourse CI](http://concourse.ci)

Resource to upload files to S3. Unlike the [the official S3 Resource](https://github.com/concourse/s3-resource), this Resource can upload or download multiple files.

## Usage

Include the following in your Pipeline YAML file, replacing the values in the angle brackets (`< >`):

```yaml
resource_types:
- name: <resource type name>
  type: docker-image
  source:
    repository: 18fgsa/s3-resource-simple
resources:
- name: <resource name>
  type: <resource type name>
  source:
    access_key_id: {{aws-access-key}}
    secret_access_key: {{aws-secret-key}}
    bucket: {{aws-bucket}}
    options: [<optional, see note below>]
jobs:
- name: <job name>
  plan:
  - <some Resource or Task that outputs files>
  - put: <resource name>
```

## Options

The `options` parameter is synonymous with the options that `aws cli` accepts for `sync`. Please see [S3 Sync Options](http://docs.aws.amazon.com/cli/latest/reference/s3/sync.html#options) and pay special attention to the [Use of Exclude and Include Filters](http://docs.aws.amazon.com/cli/latest/reference/s3/index.html#use-of-exclude-and-include-filters).

Given the following directory `test`:

```
test
├── results
│   ├── 1.json
│   └── 2.json
└── scripts
    └── bad.sh
```

we can upload _only_ the `results` subdirectory by using the following `options` in our task configuration:

```yaml
options:
  - "--exclude: '*'",
  - "--include: 'results/*'"
```
