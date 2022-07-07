# Kong Pongo action

A Github Action for running plugin tests using [Kong Pongo](https://github.com/Kong/kong-pongo).


## Usage

See the [Kong Pongo repo](https://github.com/Kong/kong-pongo) for help on Pongo commands and environments.

This action will install and configure Pongo for a specific version of [Kong Gateway](https://konghq.com) to test against. The settings will be exported as standard Pongo environment variables, such that any follow up commands will work with the same settings.


## Inputs

| input | default | required | description |
| ----- | ------- | -------- | ----------- |
| `kong_version` |  | yes | The Kong Gateway version to test your plugin against. Check the Pongo documentation for allowed formats for latest patch releases and/or nightly builds. |
| `pongo_version` | `"latest"` | yes | The Pongo version to use for testing. This can be a Pongo version tag or branch name (use `"master"` for bleeding-edge). A special case is `"latest"` which will use the latest released version. |
| `start_environment` | `"true"` | no | By default the test environment will be spun up. Set this value to `"false"` to not start the test environment. |
| `build_image` | `"true"` | no | By default the test image will be build. Set this value to `"false"` to not build the test image. |
| `license` | value of env var `KONG_LICENSE_DATA` | no | The Kong license. This is only required if you are testing a plugin against an Enterprise version of the Kong Gateway. </br>**Note**: make sure to pass it to Github as a secret! |


## Example

```yaml
# .github/workflows/test.yaml

name: "Test"
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      fail-fast: false
      matrix:
        kong_version:
        - "2.7.x"
        - "2.8.x"
        - "nightly"

    steps:
    - uses: actions/checkout@v3

    # install Pongo, build the image for the Kong version, and
    # start the environment (postgres, cassandra, etc)
    - uses: Kong/kong-pongo-action@v1
      with:
        kong_version: ${{ matrix.kong_version }}

    # Pongo commands will now work after the setup step above
    - run: pongo run
```

## History

#### Version 1 (released 7-July-2022)

- Initial version of the action
