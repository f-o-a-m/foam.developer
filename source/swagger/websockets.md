---
title: Websocket Support
---

The FOAM API supports websockets for real time notifictation systems in several topics. The client 
initiates a websocket request with the required subscription data and is responsible for terminating this subscription,
i.e. closing it manually or by some other action such as killing the process or closing the browser tab.

Let's look at an example. Say you want to receive all messages from the FOAM Token Transfer topic `ERC20TransferSubscription` where 
the transfers are going to  the address `"0x3a9bca3065b263046cef072210cdb5845b05f1a3"` or from the address `0x4d5ae15c2323748dfa6e0aabe338fb286367dbe2`. 
You should submit a `NewSubscription` message with this information  as well as a `subscriptionID`, which is a client-defined string 
which is unique to that client for the given topic. If we wish to use the `subscriptionID` `"my-token-subs-123"`, then the message would look like

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

The real content in this subscription is the `to` and `from` fields, which are lists of addresses. Semantically they are `OR`ed, meaning that if
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

EventMessage a = EventMessage
  { "event": { "to": "0x3a9bca3065b263046cef072210cdb5845b05f1a3"
             , "from": "0x4d5ae15c2323748dfa6e0aabe338fb286367dbe2"
             , "value": "0xa"
             }
  , "eventMetaData" :: EventMetaData
  }

```



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

Here is a list of all subscription `type`s and the corresponding `data`:


