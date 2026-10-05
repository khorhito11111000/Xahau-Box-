# Xahau-Box

A Xahau hook builds a box with no recipient name. Testnet.

It can hold coins only, a URIToken only, or both together. They stay on the account until the box is opened. Whoever opens it with their key takes it.

The installed hook hash C3C5FD66197D82C3B0C0EFF8744E96BA1402D6199E4CC9A3BCD5D392F8705083

## Files

- tgshook_03.c is the source.
- tgshook_03.c.wasm is the build that was installed.
- pack.json is the pack transaction. Addresses and the key fingerprint are empty.
- claim.json is the open transaction. The public key and signature are empty.
- remit.json does not install the hook. It only allows the already installed hook to emit Remit.

NetworkID 21338 is testnet.

## Pack

Signed by the account that holds the hook. CMD is PACK. VC is the key fingerprint, 32 bytes. TID is the URIToken id, or 32 zero bytes if there is no token. AMT is the coin amount in drops, or 8 zero bytes if there are no coins. EXP is the ledger when the owner may take the box back, or 4 zero bytes if there is no expiry.

An empty box is rejected. A box bigger than the coins still free on the account is rejected. A second box with the same fingerprint is rejected. Several boxes can sit on the account if the coins cover all of them. A token in a box is marked locked.

## Open

Signed by the account that wants the box. CMD is CLAIM. PK is the 33-byte public key. SIG is the 64-byte signature over the opener account and the key fingerprint. The hook hashes that message before it checks the signature. A copied signature stays tied to the account that was signed.

The private key is not in these files.

## Known gaps

A box can be packed into the account reserve. It opens, but the coins do not leave. If the emitted send fails, the box is already marked open. The token-list check can stop an unrelated send if a locked token id appears inside it.
