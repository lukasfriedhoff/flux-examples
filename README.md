# flux-examples
This example shows multiple levels of flux-kustomizations build via ```flux build``` command.

### Command

```bash
flux build kustomization flux-system \
  --kustomization-file=./gotk-sync.yaml \
  --recursive \
  --path=../flux/ \
  --local-sources=GitRepository/flux-system/flux-system=./ \
  --local-sources=GitRepository/flux-system/level1=./level1/ \
  --local-sources=GitRepository/flux-system/level2=./level1/level2/ \
  --local-sources=GitRepository/flux-system/level3=./level1/level2/level3/ \
  --verbose > ./output.yaml
```

### Explanation

1. **`--kustomization-file=./gotk-sync.yaml`**:
   - Specifies the Kustomization file to use for the build process.

2. **`--recursive`**:
   - Ensures that all dependent Kustomizations are included in the build.

3. **`--path=../flux/`**:
   - Sets the base path for the Kustomization files.

4. **`--local-sources`**:
   - Maps local directories to their corresponding `GitRepository` resources:
     - `GitRepository/flux-system/flux-system=./`: Maps the root level.
     - `GitRepository/flux-system/level1=./level1/`: Maps the first level.
     - `GitRepository/flux-system/level2=./level1/level2/`: Maps the second level.
     - `GitRepository/flux-system/level3=./level1/level2/level3/`: Maps the third level.

5. **`--verbose`**:
   - Enables detailed output for debugging and verification.

6. **`> ./output.yaml`**:
   - Redirects the output of the build process to a file named `output.yaml`.

### Output

The resulting `output.yaml` file contains the fully rendered manifests for all levels of the Flux Kustomizations, including dependencies and substitutions.

### Note
Please note, that the ```base-config.yaml``` unfortunately has to be present in the lvl3 kustomization directly and is not shared in the recursive build.

To test variable substitution, you can use kustomize build and flux envsubst via:

```
export PASSWORD="kaesebrot"
kustomize build level1/level2/level3/ |flux envsubst
```
