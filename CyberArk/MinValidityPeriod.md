The MinValidityPeriod parameter controls the number of minutes the CPM waits from the last retrieval of the password until it will attempt to change it. This gives the user a minimum period of time in which he can use the password before it is changed by the CPM. If Exclusive mode is enforced on the Safe where the password object is stored, the MinValidityPeriod parameter also determines the number of minutes after which an exclusive password will be released automatically by the CPM.

The MaxSessionDuration parameter determines the maximum duration of the session, in minutes. This can be specified as a general PSM parameter or in a specific platform.

Note: When users log off from the remote Windows machine, the sessions on both the PSM and the remote machine are ended. However, when users disconnect the session by clicking Close or if the MaxSessionDuration parameter has expired, the PSM session is automatically ended, but the session on the remote machine continues running. The next time they log onto the same remote machine through the PSM, they will continue the same session as before. To prevent this, make sure that the Terminal Server is configured to end disconnect sessions after a specific time period.

In order to set it to unlimited:

MaxSessionDuration – The maximum duration in seconds of the session. The default value is 0 (zero), indicating that the session duration is unlimited.

TL;DR (and your answers...)

Q. Does initiating a new PSM connection refresh the timer for MaxSessionDuration on a checked-out account?

A. If the user logged off properly, the MaxSessionDuration will reset since the PSM session was closed. If the user disconnected without logging off properly, they will join the same session they disconnected from and not reset the MaxSessionDuration.

Q. Does initiating a new PSM connection refresh the timer for MinValidity on a checked-out account?

A. I assume in this case a "checked-out account" is one that has Exclusive Usage enabled. In that instance, MinValidityPeriod timer starts when the account is locked and then unlocks the account after the interval. The password can be accessed as many times as needed during that time period. If MinValidityPeriod is not being used with Exclusive Usage, it will reset anytime the credential has a Show/Copy/Connect activity.

---

I will answer this for you with my understanding.

MaxSessionDuration timer is refreshed on a new PSM Connection to a previously checked out account - but only for the new session. If you have 2 sessions running utilizing the same "managed account", the old timer is still going for the first session you established.

MinValidity timer refreshed anytime a user does anything with the password, including show/copy/Connect.

If MinValidity is smaller than MaxSession duration the CPM will reset the password anyway and will not respect the MaxSessionDuration (since it's a Max). This can cause the target account to get locked if the user is currently using it.

As some additional tips:

You should also look at the setting for "ResetOverridesMinValidty"

For EA accounts, make sure you also set "Enforce One Time Password" AND give the CPM user (PasswordManager) unlock permissions in the safe. If you don't set one of those the CPM will not unlock the account once the minValidityPeriod expires.
