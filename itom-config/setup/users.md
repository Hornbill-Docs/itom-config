# Users
The list of users helps you view and manage who is using a subscription for ITOM.  Users can be added and removed from this list to provide or take away access.

## Assigning Users
Provided there are available subscriptions, the `Assign User` button can be used to allocate a subscription to a user.

Adding a user to the list will reserve a subscription for the user.  ITOM roles will need to be allocated to their user account before they can access the ITOM app.

## Removing Users
One or more users can be removed from the list by selecting the check-box next to their User ID and then clicking on the `Delete` button.

:::note
Removing a user from this list does not remove any assigned ITOM roles or rights. The subscription may be automatically reassigned the next time the user logs in if the setting `subscription.application.allocateOnLogin` is set to `ON`.
:::

## Advanced

The following platform setting will automatically add a user to this list when they have been assigned a ITOM role or right. 
|Setting|Description|Default|
|-|-|-|
|subscription.application.allocateOnLogin|When set to 'true' a user that has been granted rights to an application will automatically be granted an application subscription when a free subscription for that application is available.|ON|