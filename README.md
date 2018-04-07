# packaging-aur

This repository hosts Arch Linux `PKGBUILD` files of Pipeseroni pipes for [Arch User Repository][AUR], each is under their own branch with the same name as the pipe's:

Pipes                    | Branches
------------------------ | -------------------------
[`pipes.c`][pipes.c]     | [`pipes.c`][branch-c]
[`pipes.sh`][pipes.sh]   | [`pipes.sh`][branch-sh]

[AUR]: https://aur.archlinux.org/
[pipes.c]: https://github.com/pipeseroni/pipes.c
[pipes.sh]: https://github.com/pipeseroni/pipes.sh
[branch-c]: https://github.com/pipeseroni/packaging-aur/tree/pipes.c
[branch-sh]: https://github.com/pipeseroni/packaging-aur/tree/pipes.sh


## Packagers

See [@Pipeseroni/Packagers][packagers] for list of members who are responsible for the packaging.

[packagers]: https://github.com/pipeseroni/pipeseroni.github.io/wiki/Teams#devspackagers


## Packaging

After a new release is made of a Pipeseroni project, the following fields need
to be updated:

* `pkgver` in `PKGBUILD`.
* `sha256sums` in `PKGBUILD` and `.SRCINFO`.

### Automated packaging

The entire packaging process can be automated with
[scripts/autoupdate](scripts/autoupdate), which will iterate over each branch
in this repository and fetch release information for that project from GitHub's
API. For branches with new releases, the script will bump `pkgver` and
`sha256sums`, interactively commit, and prompt to push changes to the AUR.

The autoupdate script requires that [`jq`][jq] be installed. It also requires
`wget` and `bash`.

### Git hooks

It is recommended that you install [scripts/pre-commit](scripts/pre-commit) as
a pre-commit hook (`cp scripts/pre-commit .git/hooks`). This will automatically
update `.SRCINFO` before any commits, and will abort commits if the
`sha256sums` in `PKGBUILD` do not match the actual checksums. If this hook is
installed, you simply need to edit `pkgver`, run `updpkgsums` to update the
checksums, and commit `PKGBUILD`.

It is also recommended that you install [scripts/pre-push](scripts/pre-push).
This hook will prevent any pushes from branch names that do not match the
remote name. This will help avoid confusion arising from typos by print an
error message when, for example, trying to push to the `pipes.sh` remote when
on the `pipes.c` branch. It will also check that the `.SRCINFO` being pushed
exactly matches an autogenerated `.SRCINFO`.

Both hooks require `bash`.

### References

* [Arch packaging standards][ArchStandard]

* [Arch User Repository][Arch User Repository]

  * [Rules of submission](https://wiki.archlinux.org/index.php/Arch_User_Repository#Rules_of_submission)

* [`PKGBUILD`][PKGBUILD]
* [`.SRCINFO`][SRCINFO]
* [`makepkg`][makepkg]

[ArchStandard]: https://wiki.archlinux.org/index.php/Arch_packaging_standards
[Arch User Repository]: https://wiki.archlinux.org/index.php/Arch_User_Repository
[PKGBUILD]: https://wiki.archlinux.org/index.php/PKGBUILD
[SRCINFO]: https://wiki.archlinux.org/index.php/.SRCINFO
[makepkg]: https://wiki.archlinux.org/index.php/Makepkg
[jq]: https://stedolan.github.io/jq/


## Copyright

The files in this repository (but *not* the projects packaged by these scripts)
are released to the public domain via the [CC0 waiver](LICENSE).

