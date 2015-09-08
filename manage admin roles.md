# How to add or change Azure co-Administrator, Service Administrator and Account administrator

## Summary
This topic help you with the following task:

* Understand the different beween three admintrator roles
* Add a co-administrator for an subscription
* Change Service Administrator for a subscription
* Change the account administrator (transfer ownership of the Azure account to another account)

## Azure administrators

There are three kinds of administrator roles in Windows azure:

| Administrative role   | Limit  | Description
| ------------- | ------------- |---------------|
|Account Administrator  | 1 per Azure account  |It is the account you used to sign up or buy azure subscriptions. It is Authorized to access the Account Center (create subscriptions, cancel subscriptions, change billing for a subscription, change Service Administrator, and more)
| Service Administrator | 1 per Azure subscription  |Authorized to access Azure Management Portal for all subscriptions in the account. By default, same as the Account Administrator when a subscription is created|
|Co-administrator|200 per subscription (in addition to Service Administrator)|Same as Service Administrator, but can’t change the association of subscriptions to Azure directories.|


## Add a co-administrator for an subscription
1. Sign in to the Azure Management Portal. 
2. In the navigation pane, click Settings, click Administrators, and then click Add. ！(image)[./Media/addcoadmin.png]
