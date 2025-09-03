Support for ipfs:// URI's

Test case:
- ipfs://QmTWHDsZKruRDBYFKy7CNNR17SLkhWdvhBoiW9YJtsozeT/

Functionality:
- Requests forwarded to local IPFS gateway on http://0.0.0.0:8080 to the relevant URL http://0.0.0.0:8080/ipfs/QmTWHDsZKruRDBYFKy7CNNR17SLkhWdvhBoiW9YJtsozeT
- HTTPS is assumed
- <a href> which point to ipfs:// should be clickable (similar to mailto: scheme)

## User testing.

Open in browser: 

```txt
data:text/html,<a href="ipfs://k2jmtxww1kskig7sjqrz14excpty4wwh7yqqx6o0pi4si1zxl27o86e4/">ipfs://k2jmtxww1kskig7sjqrz14excpty4wwh7yqqx6o0pi4si1zxl27o86e4/</a>
```

`ipfs://k2jmtxww1kskig7sjqrz14excpty4wwh7yqqx6o0pi4si1zxl27o86e4` is the URL for the current instance of `vitalik.eth`.

## TODO.

- [x] Implement ipfs: URI scheme support.
- [ ] Disable omnibox when routing to ipfs: URI's.
- [ ] When visiting `ipfs:` URI's, all relative links (ie. from a base page of ipfs://Qma.., links that are /post/xxx) currently go to the gateway at http://0.0.0.0/ipfs/Qma...2

