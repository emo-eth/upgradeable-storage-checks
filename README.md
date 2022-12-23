

## Generating a cheatcode-compatible storage file

`forge inspect <contract_name> storage | jq '.["storage"] |= map(. + {_contract: .contract, _type: .type} | del(.contract, .type))' | jq '{_storage: .storage}' > storage_layouts/<contract_name>_latest.json`