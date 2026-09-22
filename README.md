# gro-nass — the record's chain head, published

This repository exists to be a witness and holds nothing else.

Every night the gro-nass record is walked from row 1 to its head: every hash checked for form, every
row's `prev_hash` checked against the row before it, every signature verified against the key
enrolled for that sender. `head.txt` carries the resulting digest and the date it was taken.

`head.txt.sig.b64` is an ed25519 signature over the exact bytes of `head.txt`, made with the same
owner key the record's own rows are signed with. Its public half is `owner.pub`, and the same key
appears as `owner_pub` in every bundle that repository has ever sent home.

To check it:

    node -e 'const c=require("crypto"),f=require("fs");
      console.log(c.verify(null, f.readFileSync("head.txt"),
        c.createPublicKey(f.readFileSync("owner.pub")),
        Buffer.from(f.readFileSync("head.txt.sig.b64","utf8").trim(),"base64")))'

## What this does and does not prove

It proves that on the date in the file, this digest was published somewhere neither we nor a reader
can quietly edit — GitHub keeps the commit history and the push timestamps, and a value changed
later is visible as a change.

It does not prove the rows behind the digest are true. A signature binds a `row_hash`, and a
`row_hash` covers bytes the receiving end never sees, so a forged verdict inside a whitelisted row
is not detectable from the record alone. That limit is stated on the site and in the format
document, and closing it needs the sender to sign the projection as well.

The site is https://gro-nass.com and the format is at https://gro-nass.com/docs/FORMAT.md
