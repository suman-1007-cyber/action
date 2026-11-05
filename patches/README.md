# patches/

Drop `*.patch` files here (git-apply format). The workflow applies them in alphabetical order
before KernelSU setup and the build; a patch that fails `git apply --check` is skipped with a warning.

Your tree already contains SUSFS + NoMount and uses KPROBES for KernelSU hooks, so no
manual-hook patch is needed (KSU_HOOKS_PATCH=false). If you later switch to manual hooks,
set KSU_HOOKS_PATCH=true and add the hook patch here as `10-ksu-hooks.patch`.
