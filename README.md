# Mage Web API Documentation
[Mage](https://www.getmage.io/) is a price optimization tool for in-app purchases.<br>
Forget continuously setting up A/B-Tests or hiring data science teams for your app pricing! Mage fully automates simultaneous price testing & setting for in-app purchases in over 170 countries separately. So you don't have to deal with it!<br>
Get started with [Mage](https://www.getmage.io/) to scale your products worldwide!

This is a documentation of the Mage Web API for your backend.<br>
The API is documented according to the [openapi3.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.0.md) standard.

# Recommended Usage
The Mage Web API is kept quite simple. A more beautiful (designed) description can be found [here](https://hub.apitree.com/FlyingLasagna/privatecheck/). Sadly APITree does not show all openAPI3.0 definitions, so please have a look at ***this*** open source GitHub repository for complete documentation.

The servers URL is: `https://room-of-requirement.getmage.io`<br>
Current Version: 1.0.4<br>
Version Path: `/v1`<br>
There is one path available: `/aparecium`<br>
(Yeah we like magic, don't you?)<br>

## Authorization
For authorization, you set your personal Web-API-Key in your request header as `Token`.
You can access your personal Web-API-Key in the Mage Web App.

## Submit In-App Purchase Status Update (especially Subscriptions)
Status updates, like for example for subscriptions, can not be tracked by your app itself. Subscription status tracking is usually done on your backend or by some third party service. Apple or Google sends real-time subscription status updates that you interpret and take action on. This is why we provide a simple Web API to enable subscription lifetime value tracking for Mage. Apple or Google contacts your backend, your backend contacts Mage.
To submit a status change, use the path `/aparecium`. We distinguish between four fundamental status changes: Renewing a subscription, switching a subscription, refunding an In-App purchase (IAP) and rebuying an IAP. The `iapIdentifier` as the identifier for the IAP, the `appUserId` as the id you identify your users' subscription status with, the `subChangeState` declaring the status type update as well as the `stateChangeTime` stating the datetime of the status change are mandatory attributes. The `subChangeState` can take four distinguished strings to define the status change: *'RENEW'*, *'SWITCH'*, *'REFUND'* or *'REBUY'*. To submit a switch of subscription you additionally need to define the `newIapIdentifier` so we can identify the following subscription.

### Path
`https://room-of-requirement.getmage.io/v1/aparecium`

### Request Object
|Name     |Data Type    |Description    |
|---  |---  |---  |
|iapIdentifier      |string  |The In-App Purchase Identifier of the IAP, of which the status was updated.    |
|appUserId          |string  |The User Id of the user who did the status update. Watch out, to use the same user id, which you set for the Mage SDK in your App!     |
|subChangeState     |string  |The type of status update. Currently there are 4 types supported: 'Renew', 'Refund', 'Switch' and 'Rebuy'.     |
|stateChangeTime    |long	   |The DateTime at which the change has been done.     |
|newIapIdentifier   |string  |Only for 'Switch' status changes: the In-App Purchase Identifier of the IAP, to which the user switched. (So that the 'iapIdentifier' object is the old IAP)     |

### Response Object
|Name     |Data Type    |Description    |
|---  |---  |---  |
|message        |string    |     |
|status         |long      |     |

---

## Models

---

## Errorcodes
### Wrong API Usage
|Error     |Name     |Description    |
|---  |---  |---  |
|3001      |Attribut parsing missmatch      |Attribute could not be parsed, check the attribute types.     |
|3002      |Missing Attribut      					|A required Attribute is missing, check the required attributes and/or control attribute naming.     |
|3021      |Unknown Subscription State      |Attribute could not be parsed, check the attribute types.     |

---

## Version History
