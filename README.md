# Policies

This repo contains a library of constraint templates and sample constraints.

For information on setting up Config Validator to secure your environment, see the [User Guide](./docs/user_guide.md).

## Developing a Constraint

If this library doesn't contain a constraint that matches your use case, you can develop a new one
using the [Constraint Template Authoring Guide](./docs/constraint_template_authoring.md).

### Available Commands

```
make audit                          Run audit against real CAI dump data
make build                          Format and build
make build_templates                Inline Rego rules into constraint templates
make debug                          Show debugging output from OPA
make format                         Format Rego rules
make help                           Prints help for targets with comments
make test                           Test constraint templates via OPA
```

### Inlining
You can run `make build` to automatically inline Rego rules into your constraint templates.

This is done by finding a `INLINE("filename")` and `#ENDINLINE` statements in your yaml,
and replacing everything in between with the contents of the file.

For example, running `make build` would replace the raw content with the replaced content below

Raw:
```
#INLINE("my_rule.rego")
# This text will be replaced
#ENDINLINE
```

Replaced:
```
#INLINE("my_rule.rego")
#contents of my_rule.rego
#ENDINLINE
```

### Linting Policies
FCV provides a policy linter.  You can invoke it as:

```
go get github.com/forseti-security/config-validator/cmd/policy-tool
policy-tool --policies ./policies --policies ./samples --libs ./lib
```

### Local CI
You can run the cloudbuild CI locally as follows:

```
gcloud components install cloud-build-local
cloud-build-local --config ./cloudbuild.yaml --dryrun=false .
```

### Updating CI Images

You can update the CI images to add new versions of rego/opa as they are released.
```
# Rebuild all images.
make -j ci-images

# Rebuild a single image
make ci-image-v1.16.0
```

