# Rust library crate Release Checklist
Bart Massey 2023

The list below is what I currently use when releasing or
re-releasing a Rust library crate. It's just scribbled
notes: use advisedly.

# License

This work is licensed under the "MIT License". Please see the file
`LICENSE.txt` in this distribution for license terms.

-----

# First published release

* If the root branch of the repo is `master`, rename it to
  `main` using the Github UI and also rename it locally.
* Add Rustdoc root to the crate root

        #![doc(html_root_url = "https://docs.rs/<crate>/<version>")]

  (where `<crate>` is the crate name and `<version>` is the
  version name)
* Review the Rust API Guidelines
  [checklist](https://rust-lang.github.io/api-guidelines/checklist.html).
  Note that if the `documentation` field of `Cargo.toml` is
  unset, the docs will appear on `crates.io` anyhow once
  processed.
* Remove any `publish=false` from `Cargo.toml`
* Add `version-sync` to `dev-dependencies` and install the
  `version-sync.rs` test in `tests/`
* Deal with the badge display mess. Add some badges to
  `README.tpl` or `README.md`. If `README.tpl`, do not use
  `cargo-readme`'s badge feature, which is currently kind of
  busted.

  Start by inserting `badges.md` from here. If using
  `README.md` directly, replace all instances of `{{crate}}`
  with the crate name. If you are not `BartMassey` or using
  `github`/`github.com`, fix that too. In general, edit to
  taste.
* Add maintenance badge to `Cargo.toml`

        [badges.maintenance]
        status = "actively-developed"

* Set up CI. Easiest is probably to click through the Github
  UI to add the default Rust action; don't forget to update
  the action name from Rust to CI. Generate a CI badge there
  and add it to `README.md` or `README.tpl` if you don't
  have one already.
* Continue with the per-release instructions.

# Every release

* Update `Cargo.toml` version
* Update `html_root_url` version in crate root
* Update `README` version
    * If using `cargo-readme`, run it
    * Otherwise update manually
* Run `showmd README.md` to make sure it looks OK
* Run `cargo doc --open` and check that everything
  looks sane
* Grep for the old version number to see if anything
  has been left lying around.
* Run `cargo fmt --all` to check
* Run `cargo clippy --all` to check
* Run `cargo test` to check `version-sync`
* Commit the new version.
* `git tag-release` to tag the version
* Push the release and make sure it looks OK on Github
* If local documentation is also provided, update that. (I
  use a shell script called `rustdoc-release` for this.)
* Wait for Github CI and/or Pages to finish
* Publish to `crates.io` with `--dry-run` and make sure it works
* Publish to `crates.io`
* Wait for `crates.io` to process, and check everything out
