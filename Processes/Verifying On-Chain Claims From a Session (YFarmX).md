---
tags: [process, yfarmx, research, crypto]
source: verifying the Tornado Cash root-history claim on 2 and 3 September 2026, including the wrong conclusion drawn first
updated: 2026-09-03
---

# Verifying On-Chain Claims From a Session (YFarmX)

A crypto desk publishes claims about deployed contracts. Those claims are checkable, cheaply, from a session — and on 2 September 2026 a session concluded they were not, told Jay so, and was wrong. This note exists so that does not repeat.

## Etherscan's website is blocked; almost nothing else is

Through the session proxy:

- `etherscan.io` (the HTML site) returns **403**. It is behind bot protection. You cannot screenshot an address page.
- Public JSON-RPC endpoints **work**. `ethereum-rpc.publicnode.com` and `eth-pokt.nodies.app` both answered correctly. `cloudflare-eth.com` returned an internal error, `rpc.ankr.com` wants an API key, `1rpc.io` and `eth.llamarpc.com` were down. **So try several before concluding anything.**
- `raw.githubusercontent.com` and `home.treasury.gov` work.

The first attempt hit three endpoints, got a 403 from each, and concluded "public RPCs are blocked". Two of those three were simply the wrong endpoints, and the failure was not the proxy. **A 403 from one host is not a statement about a class of hosts.**

## Read the deployed contract, not the repository

The claim under test: Tornado Cash's pools keep **100** historical Merkle roots, while the published source says 30.

`ROOT_HISTORY_SIZE` is declared `uint32 public constant` in `MerkleTreeWithHistory.sol`, and it is still `30` on master today (line 33, verified via raw.githubusercontent.com). All four ETH pools return **100**:

```python
from Crypto.Hash import keccak
import json, urllib.request
k = keccak.new(digest_bits=256); k.update(b"ROOT_HISTORY_SIZE()")
sel = "0x" + k.hexdigest()[:8]                     # 0xcd87a3b4
body = json.dumps({"jsonrpc":"2.0","id":1,"method":"eth_call",
                   "params":[{"to":POOL,"data":sel},"latest"]}).encode()
r = urllib.request.Request("https://ethereum-rpc.publicnode.com", data=body,
                           headers={"content-type":"application/json"})
int(json.loads(urllib.request.urlopen(r, timeout=25).read())["result"], 16)   # 100
```

`pip install pycryptodome` gives you keccak for the selector. Confirmed on two independent nodes across all four pools (100, 10, 1 and 0.1 ETH).

## What that discrepancy actually means, and the framing it forces

A Solidity `constant` is **inlined into the bytecode at compile time**. It cannot be changed after deployment and cannot differ from the source it was compiled from. So deployed-100 against source-30 has exactly one explanation: **the deployed contracts were compiled from different source than the repository's current master.** The repository drifted from what is running.

That matters editorially. "The docs are wrong" is the weaker, vaguer claim and invites a correction. "The published repository no longer matches the deployed bytecode, and here is the call that proves it" is stronger, verifiable by any reader, and is what the language of the contract actually supports.

**The general rule: when the chain disagrees with a repository, work out whether the value CAN change at runtime before writing what the disagreement means.** A `constant` narrows it to one explanation; a storage variable would leave several.

## Related

- [[Verify before you correct published copy]] — a finding is evidence, not a verdict
- [[Screenshotting external sites from a Claude session (YFarmX)]]
- [[YFarmX]]
- [[Map - Processes]]
