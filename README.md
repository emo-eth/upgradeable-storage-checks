# Upgradeable Storage Checks

Included are a set of proof-of-concept tests and tools for using `forge inspect` and Forge's filesystem cheatcodes to do basic checks for incompatible storage updates among versions of an upgradeable implementation.

**Note** that it currently does not account for fixed-length array types, ie, if a fixed-length array is shortened but has a dirty slot, and a new storage variable replaces that slot, the test will not pick that up (yet).

**Also note** that it will break if private variables are introduced between two private variables from inherited contracts with the same name, eg, a new `__gap` variable is introduced in slots between two other private `__gap` variables.


## Usage

### `watched_contracts.txt`

A list of contract names (e.g. "ExampleFile") that we should check layouts of. There should be one name per line, and end with a newline. The other helper scripts will do their best to append a newline to the file.

### `generate_latest_layouts.sh`

A bash script that generates `"<contract name>_latest.json"` files for each contract included in `watched_contracts.txt`. Layouts are written to the `storage_layouts` folder. These will be compared against the `"<contract name>.json"` files.

Run with `bash generate_latest_layouts.sh`

### `copy_latest_layouts.sh`

A bash script that copies the contents of the `"<contract name>_latest.json"` files to versions without the `_latest` suffix. This effectively "finalizes" a new storage layout once committed.

Run with `bash copy_latest_layouts.sh`

### `StorageLayout.t.sol`

This test reads from `watched_contracts.txt` and tries to load the appropriate JSON files to compare the storage layouts. The tests will fail if two associated `label`s `slot`s or `offset`s do not match.

