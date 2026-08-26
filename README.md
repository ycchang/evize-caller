# evize-caller

Calls [enclavize](https://github.com/hylswind/evize-workflow) against a
sacrificial AWS account.

Nothing here does any work. The whole point of a caller is that the code which
seals the account lives in the reusable workflow, pinned, and this repo only
supplies the inputs and the secrets. The attestation lands here because this is
where the run executes.

Trigger it with:

    gh workflow run enclavize.yml -R hylswind/evize-caller \
      -f domain=example.com \
      -f start=$(( $(date +%s) - 3600 )) \
      -f repo=owner/app

Verify what it produced:

    gh attestation verify statement.json \
      --repo hylswind/evize-caller \
      --signer-workflow hylswind/evize-workflow/.github/workflows/enclavize.yml \
      --predicate-type https://enclavize.dev/enclaved-account/v1
