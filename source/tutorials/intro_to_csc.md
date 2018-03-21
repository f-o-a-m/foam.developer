title: Intro to CSC
---

# Crypto-spatial coordinates

The most fundamental function of the `CSC` contract is to make an immutable pairing between a physical address (encoded as a [geohash](https://en.wikipedia.org/wiki/Geohash) and an Ethereum contract address.

For details behind the reasoning, see our [blog post](https://blog.foam.space/crypto-spatial-coordinates-fe0527816506).

Below is the skeleton of the `CSC` contract:


```javascript
contract CSC {
    bytes8 public geohash;
    bytes12 public csc;

    function CSC(bytes8 _geohash) {
      geohash = _geohash;
      csc = computeCSC(geohash, address(this));
    }

    function computeCSC(bytes8 geohash_arg, address addr) internal returns(bytes12) {
      return bytes12(sha3(geohash_arg, addr));
    }
}
```

To keep track of all `CSC`'s, we utilize a registry:

```javascript
contract CSCRegistry {
  event RegisterCSC(bytes12 csc, address addr, bytes8 geohash);
  mapping(bytes12 => address) public registry;

  function register(bytes12 csc) {
    if (registry[csc] == 0) {
      require(computeCSC(caller.geohash(), msg.sender) == csc);
      registry[csc] = msg.sender;
      RegisterCSC(csc, msg.sender, caller.geohash());
    }
  }

  function computeCSC(bytes8 geohash, address addr) internal returns(bytes12) {
    return bytes12(sha3(geohash, addr));
  }
}
```

When a `CSC` gets registered in the `CSCRegistry`, an event named `RegisterCSC` is triggered. On the backend, we're parsing these events and ultimately let the developer query them using our API or to subscribe to them using websockets.
