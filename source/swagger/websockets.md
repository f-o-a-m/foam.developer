---
title: Websocket Support
---

The FOAM API supports websockets in several topics for real time notifictation systems. The client 
initiates a websocket request with the required subscription data and is responsible for terminating this subscription,
e.g. by closing it manually or by some other action such as killing the process or closing the browser tab.

Let's look at an example. Say you want to receive all messages from the FOAM Token Transfer topic `ERC20TransferSubscription` where 
the transfers are going to the address `"0x3a9bca3065b263046cef072210cdb5845b05f1a3"` or from the address `0x4d5ae15c2323748dfa6e0aabe338fb286367dbe2`. 
You should submit a `NewSubscription` message with this information  as well as a `subscriptionID`, which is a client-defined string 
unique to that client for the given topic. If we wish to use the `subscriptionID` `"my-token-subs-123"`, then the message would look like

### New Subscription
```json
{ "data": { "data": {"update": { "to": ["0x3a9bca3065b263046cef072210cdb5845b05f1a3"]
                               , "from": ["0x4d5ae15c2323748dfa6e0aabe338fb286367dbe2"] 
                               }
                    ,"subscriptionID": "my-token-subs-123"
                    }
          ,"type":"NewSubscription"
          }
, "type":"ERC20TransferSubscription"
}	

```

The real content in this subscription is the `to` and `from` fields, which are lists of ethereum addresses. Semantically they are `OR`ed, meaning that if
one is satisfied -- i.e. the transfer recipient is in the `to` list or the sender is in the `from` list -- then the topic matches. For instance,
if you wanted additionally to track transfers from `"0xe76790d814e7f331ff645551818e182eeb70fbb1"`, you would submit 
```json
...
, "from": ["0x4d5ae15c2323748dfa6e0aabe338fb286367dbe2", "0xe76790d814e7f331ff645551818e182eeb70fbb1"] 
...
```
They are  also optional -- for example, if you submit `null` for `to` then you will match any `to` address. 

The incomming message will have the following format:

```json

{ "data": { "message": { "event": { "to": "0x3a9bca3065b263046cef072210cdb5845b05f1a3"
                                  , "from": "0x4d5ae15c2323748dfa6e0aabe338fb286367dbe2"
                                  , "value": "0xa"
                                  }
                       , "eventMetaData": { "blockHash": "0xbbac565543750aeab5dfc14a5dddef3226a4e10f5e3ddc584caf7d34145426c3"
                                          , "address": "0x6f5eae0a65d2da608f4ff5d9676fe61fd139216b"
                                          , "blockNumber":"0xeeb21"
                                          , "id": "0xc718b9940dc3e6e6cfae48be211e0faf008e5bb5bba8f9beb5d72968573921e5"
                                          , "txHash": "0x3a0f64edc8539bd0bf590d06f5ccd46d58991a9bd1ecd974e2b47813a190056f"
                                          }
                       }, "subscriptionID": "my-token-subs-123"
          }
, "type": "ERC20TransferMsg"
}	
```

meaning that 10 tokens were transfered. The `eventMetaData` field contains meta information about this event, such as the transaction hash, block number, etc.

If you would like to change the subscription, you may simply send a message with the same format as the original, using the same subscription-ID. 
When we are ready to stop receiving messages from this topic, we can submit a cancel message containing the `subscriptionID`

### Cancel Subscritpion
```json
{ "data": { subscriptionID: "my-token-subs-123"
          }
, "type": "CancelSubscription"
}

```

All subscriptions follow the same format as above, the only fields that change are the `type` field in the `NewSubscription` message 
(`ERC20TransferSubscription` in the above example) and the inner `data` field (`{to :: Address, from :: Address}` in the above example).

The format of the messages are easy to deduce from the above example, the only changes are 
 1. The message `type`, e.g. `"ERC20TransferSubscription", "ERC20TransferMsg"` which generalize to `*Subscription` and `*Msg` respectively.
 2. The `data.data.update` object in the `NewSubscription` messages from client to server.
 3. The `data.message.event` object in the messages from server to client.
 
Here is a list of all subscription `type`s and the corresponding `data`:

## FOAM Token Events

### Transfer
```
- type: "ERC20Transfer"
- data.data.update:
    { from :: [Address]
    , to :: [Address]
    }
- data.message.event: 
    { from :: Address
    , to :: Address
    , value :: HexInteger
    }

```

### Approval
```
- type: "ERC20Approval"
- data.data.update:
    { owner :: [Address]
    , spender :: [Address]
    }
- data.message.event: 
    { owner :: Address
    , spender :: Address
    , value :: HexInteger
    }

```

## PLCR Events

### Vote Committed
```
- type: "PLCRVoteCommitted"
- data.message.update:
    { voter :: [Address]
    , pollID :: [HexInteger]
    }
- data.message.event:
    { voter :: Address
    , pollID :: HexInteger
    , numTokens :: HexInteger
    }
```

### Vote Revealed
```
- type: "PLCRVoteRevealed"
- data.message.update:
    { voter :: [Address]
    , pollID :: [HexInteger]
    }
- data.message.event:
    { voter :: Address
    , pollID :: HexInteger
    , numTokens :: HexInteger
    , choice :: HexInteger
    }
```

### Voting Rights Update
```
- type: "PLCRVotingRightsUpdate"
- data.message.update:
    { voter :: [Address]
    }
- data.message.event:
    { voter :: Address
    , numTokens :: HexInteger
    , votingRights :: "Granted" | "Withdrawn"
    }
```

### Tokens Rescued
```
- type: "PLCRTokensRescued"
- data.message.update:
    { voter :: [Address]
    , pollID :: [HexInteger]
    }
- data.message.event:
    { voter :: Address
    , pollID :: HexInteger
    }
```

## TCR Events

### Valid Application
```
- type: "RegistryValidApplication"
- data.message.update:
    { listingHash :: [HexString]
    , owner :: [Address]
    }
- data.message.event:
    { listingHash :: HexString
    , deposit :: HexInteger
    , endDate :: UTCTime
    , data :: String
    , pOIData :: POIData
    , owner :: Address
    , createdAt :: UTCTime
    }
```

### Invalid Application
```
- type: "RegistryInvalidApplication"
- data.message.update:
    { listingHash :: [HexString]
    , owner :: [Address]
    }
- data.message.event:
    { listingHash :: HexString
    , deposit :: HexInteger
    , endDate :: UTCTime
    , data :: String
    , owner :: Address
    , createdAt :: UTCTime
    }
```

### Update Listing Status
```
- type: "RegistryUpdateListingStatus"
- data.message.update:
    { listingHash :: [HexString]
    }
- data.message.event:
    { listingHash :: HexString
    , maturity :: "Application" | "Listing" | "Removed"
    }
```

### Reward Claimed
```
- type: "RegistryRewardClaimed"
- data.message.update:
    { listingHash :: [HexString]
    , pollID :: [HexInteger]
    }
- data.message.event:
    { voter :: Address
    , pollID :: HexInteger
    , reward :: HexInteger
    }
```

### Withdrawal
```
- type: "RegistryWithdrawal"
- data.message.update:
    { listingHash :: [HexString]
    , pollID :: [HexInteger]
    }
- data.message.event:
    { voter :: Address
    , pollID :: HexInteger
    , reward :: HexInteger
    }
```

### Deposit
```
- type: "RegistryDeposit"
- data.message.update:
    { listingHash :: [HexString]
    }
- data.message.event:
    { listingHash :: HexString
    , added :: HexInteger
    , newTotal :: HexInteger
    }
```

### Challenge
```
- type: "RegistryChallenge"
- data.message.update:
    { listingHash :: [HexString]
    , owner :: [Address]
    }
- data.message.event:
    { listingHash :: HexString
    , pollID :: HexInteger
    , deposit :: HexInteger
    , data :: String
    , challengeData :: Maybe ChallengeData
    , owner :: Address
    , createdAt :: UTCTime
    }
```

### Challenge Failed
```
- type: "RegistryChallengeFailed"
- data.message.update:
    { listingHash :: [HexString]
    , pollID :: [HexInteger]
    }
- data.message.event:
    { listingHash :: HexString
    , pollID :: HexInteger
    , rewardPool :: HexInteger
    , totalTokens :: HexInteger
    }
```

### RegistryChallengeSucceeded
```
- type: "RegistryChallengeSucceeded"
- data.message.update:
    { listingHash :: [HexString]
    , pollID :: [HexInteger]
    }
- data.message.event:
    { listingHash :: HexString
    , pollID :: HexInteger
    , rewardPool :: HexInteger
    , totalTokens :: HexInteger
    }
```
