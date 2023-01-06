# A General Development Container Specification
**Problem**: How to set up a reasonably individualized dev experience on any
remote machine.

# Usage
To be used in a way that is similar to the micromamba container.
[mamba-org/micromamba-docker](https://github.com/mamba-org/micromamba-docker)

Pass in an `env.yml` file:
```yaml
# Using an environment name other than "base" is not recommended!
    # Read https://github.com/mamba-org/micromamba-docker#multiple-environments
    # if you must use a different environment name.
    name: base
    channels:
      - conda-forge
    dependencies:
      - pyopenssl=20.0.1
      - python=3.9.1
      - requests=2.25.1
```

```Dockerfile
COPY --chown=$MAMBA_USER:$MAMBA_USER env.yml /tmp/env.yaml
RUN micromamba install -y -n base -f /tmp/env.yaml && \
    micromamba clean --all --yes
ARG MAMBA_DOCKERFILE_ACTIVATE=1  # (otherwise python will not be found)
```

# TODOs
- [ ] What is the best way to include personalized dotfiles in the development
process? Certain solutions recommend a separate repository for dotfiles
