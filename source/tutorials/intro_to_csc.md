title: Intro to CSC
author: @kejace
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

# Factory pattern

A convenient way to create many `CSC`s is to employ the [factory pattern](https://en.wikipedia.org/wiki/Factory_method_pattern).

```javascript
contract Factory {
  address public factoryCSCR;
  event DeployedCSC(string _name, address _address, address _factoryAddress, bytes8 _geohash);

  function Factory (address _factoryCSCR) {
      factoryCSCR = _factoryCSCR;
  }

  function deploy(bytes8 _geohash) {
      CSC blol = new CSC(_geohash);
      beacon.register(factoryCSR);
      DeployedCSC(beacon, address(this), _geohash);
  }
}
```

The `Factory` contract keeps track of both the registry and serves as a central point of deployment. It has the benefit that it is easy to track all `CSC`'s of a certain type: just look for all `CSC`'s that were deployed via the `Factory`.

A later tutorial will go into detail how to query the API for `CSC`'s filtered by a specific address.
