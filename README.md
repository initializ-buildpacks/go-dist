# Go Distribution Cloud Native Buildpack

The Go Distribution CNB delivers the Go binary distribution essential for executing [Go tooling](https://golang.org/cmd/go/). This buildpack seamlessly installs the Go binary distribution onto the `$PATH`, ensuring its accessibility for subsequent buildpacks. Leveraging this distribution, buildpacks can effortlessly execute Go tooling tasks, including the compilation of Go application binaries.

## Integration

The Go Distribution CNB serves as a supplier of Go as a dependency. Subsequent buildpacks, such as [Go Mod](https://github.com/initializ-buildpacks/go-mod), can specify the requirement for the Go dependency by generating a [Build Plan TOML](https://github.com/buildpacks/spec/blob/master/buildpack.md#build-plan-toml) file structured similarly to the following:

```toml
[[requires]]

  # The dependency name is "go," an integral part of the buildpack's public API,
  # ensuring stability unless deprecated.
  name = "go"

  # The version of the Go dependency is optional. If not specified, the buildpack
  # defaults to the version indicated in the buildpack.toml file.
  # To request a specific version, utilize semver constraints such as "1.*", "1.13.*",
  # or even "1.13.9".
  version = "1.13.9"

  # The Go buildpack offers additional non-mandatory metadata options.
  [requires.metadata]

    # Enabling the build flag ensures the Go dependency is accessible
    # on the $PATH for subsequent buildpacks during their build phase.
    # Set this flag to true if your buildpack requires Go during its build process.
    build = true

    # Enabling the launch flag ensures the Go dependency is available
    # on the $PATH for the running application.
    # Set this flag to true if your application needs to execute Go at runtime.
    launch = true
```

## Usage

To package this buildpack for distribution:-

```
$ ./scripts/package.sh
```

By default, the build script packages the buildpack's Go source using GOOS=linux.
You can specify another value as the first argument to package.sh.


## Go Build Configuration

To configure the Go version, utilize the BP_GO_VERSION environment variable
during build time, either directly (e.g., pack build my-app --env BP_GO_VERSION=~1.14.1)
or through a project.toml file.`
file](https://github.com/buildpacks/spec/blob/main/extensions/project-descriptor.md).
