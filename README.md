# bun frozen lockfile issues

It seems that bun complains about `lockfile had changes, but lockfile is frozen` during `bun install --frozen-lockfile --ignore-scripts`. 
The `--ignore-scripts` is optional but I have `esbuild` in this repo, which would break the install step otherwise. 

I tested this on a MacStudio (M1), on a MacBook Pro (M1) and inside of a default Docker container as mentioned [in bun's docs](https://bun.sh/guides/ecosystem/docker) with **bun** `1.0.16` and bun `1.0.17`

You can also add / remove other packages, it doesn't have to be `mysql`. But there is a certain point when bun's install script starts to fail with `--frozen-lockfile`. Maybe it's due to some hashing issues when a string becomes too long? It's also reproducible with dependencies of workspaces but you might have to remove / add more packages to trigger the issue.

I also noticed that `peerDependencies` do not seem to have an impact.

## Error case

```sh
# build
docker build --platform=linux/arm64 --pull -t bun-hello-world .
``` 
❌ should fail with `error: lockfile had changes, but lockfile is frozen`

## Success case

```sh
# remove mysql dependency
bun remove mysql

# build
docker build --platform=linux/arm64 --pull -t bun-hello-world .
``` 

✅ should succeed


