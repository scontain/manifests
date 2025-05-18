# Manifests

The [manifest repository](https://github.com/scontain/manifests) contains files required by [`kubectl provision`](https://sconedocs.github.io/5_kubectl/):

- the **default manifests** for the [**SCONE custom resources**](https://sconedocs.github.io/3_resource_definitions/), and
- their associated default [**SCONE policies**](https://sconedocs.github.io/CAS_session_lang_0_3/).

The manifests are versioned using a directory named `$VERSION` for each SCONE `$VERSION`. We intend to keep these versions immutable. New features and patches will result in a new version.

Directory `templates` contains some generic manifests and policies. These are customized for each version.

Note that all remote manifests and all container images must be properly signed by scontain: we verify if these signatures. 

## Customization

In many cases, one might just stick with the defaults. Of course, they can be customized to your needs.

Our `kubectl` plugin (aka [`kubectl provision`](https://sconedocs.github.io/5_kubectl/)) uses these manifest and policies by default. This plugin will set the following variables:

- `${IMAGE}`: The container image of the custom resource (full path).
- `${IMAGE_PREFIX}`: A path in which the. Example: `your_project_id`. The default is empty.
- `${IMAGE_REPO}`: The container image repository in which the container images are stored. The default is `registry.scontain.com/scone.cloud`
- `${NAME}`: The name of the custom resource, e.g., the name of the CAS custom resource.
- `${NAMESPACE}`: The Kubernetes namespace in which this custom resource is created.
- `${OWNER_ID}`: Each Vault instance is assigned a random **owner id**, this could be seen as a random SCONE CAS policy namespace that is assigned to it.
- `${PLATFORM_IDS}`: is replaced by a list of public keys identifying the nodes of the Kubernetes cluster
- `${POLICY_NAME}`: The associated SCONE CAS policy.
- `${SCONE_CAS_ADDR}`: The name of the CAS instance that stores the policy.
- `${SCONE_CLI_MRENCLAVE}`: is replaced by the MrEnclave of the SCONE CLI.
- `${VERSION}`: the version of SCONE. Example: `5.8.0`

Note that you can use both `$VARIABLE` as well as `${VARIABLE}`. The latter is typically used to separate the `VARIABLE` name from whatever text might follow.

## Remote vs Local Manifests

These manifests and policies are all signed. The plugin verifies the signature during the download. It will abort in case the signature is missing or cannot be verified. The public key of the signature is
`D46E9525781BCFF33FFB947A28092CF463D82E8B`.

To customize the images, one can download the manifests. Please verify the signature when doing so. If you downloaded file `$file`, please check that `gpg` repos a valid signature:

```bash
gpg  --verify --status-fd=1 --verify "$file.asc" "$file" 2> /dev/null | grep 'VALIDSIG D46E9525781BCFF33FFB947A28092CF463D82E8B'
```

Local manifests and policies do not need to be signed:  `kubectl provision` only verifies the signatures when downloading files but not when using local files: we assume that you trust files on your local computer.

The modified manifests can be passed to  `kubectl provision` using option `-f`.
