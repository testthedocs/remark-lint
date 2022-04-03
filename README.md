# Docker image for remark-lint

![Build Status](https://github.com/zemanlx/remark-lint/actions/workflows/docker-publish.yml/badge.svg)

Docker image for markdown code style linter [remark-lint](https://github.com/remarkjs/remark-lint).

## Get

Pull image from [GitHub Container Registry](https://ghcr.io/zemanlx/remark-lint)

```bash
docker pull ghcr.io/zemanlx/remark-lint
```

## Build

Clone this repository and run

```bash
docker build -t ghcr.io/zemanlx/remark-lint .
```

## Use

Go to your folder with markdown files you want to lint and run

```bash
docker run --rm -i -v $PWD:/lint/input:ro ghcr.io/zemanlx/remark-lint .
```

You can even set it up as an alias for remark-cli's command `remark`.

```bash
alias remark="docker run --rm -i -v $PWD:/lint/input:ro ghcr.io/zemanlx/remark-lint"

remark --version
remark: 14.0.2, remark-cli: 10.0.1
```

Default config is `.remarkrc.yaml`

```yaml
plugins:
  preset-lint-consistent:
  preset-lint-markdown-style-guide:
  preset-lint-recommended:
  validate-links:
```

### Example

```bash
docker run --rm -i -v $PWD:/lint/input:ro ghcr.io/zemanlx/remark-lint .

README.md
  3:100  warning  Line must be at most 80 characters  maximum-line-length         remark-lint
    8:1  warning  Remove 1 line before node           no-consecutive-blank-lines  remark-lint

⚠ 2 warnings
```

### Rule Customisation

In case you want to customize rules like `maximum-line-length` you can find
examples of configuration in JSON and YAML in the `examples` folder. All you
need to do is copy it to your project root and customize.

See list of all rules in [remark-lint/packages](https://github.com/remarkjs/remark-lint/tree/master/packages).

### Continuous Integration

An option `-f` or `--frail` can be useful for exiting with code `1` in case of
any warning in your CI.

```bash
docker run --rm -i -v $PWD:/lint/input:ro ghcr.io/zemanlx/remark-lint --frail .
```

#### Travis CI

Your minimal configuration can look like

```yaml
# Docker is required to run the linter
sudo: required
services:
  - docker
# Use your language or generic image to cut start-up time
language: generic
install:
  - docker pull ghcr.io/zemanlx/remark-lint
script:
  - docker run --rm -i -v $PWD:/lint/input:ro ghcr.io/zemanlx/remark-lint --frail .
```

You can extend this example with your build instructions and tests or add it
as a job to one of your stages.
