Title
    Understanding the possible One Time Password, Exclusive and Allow Manual Change combinations

Question
    What behavior should be exhibited from the different one-time password, exclusive and allow manual change combinations?

Answer
    Listed below are the possible combinations and the behavior that can be expected. 

    One-time password, exclusive & allow manual change:
    - Account is locked when retrieved.
    - If user releases manually, the account is set for ResetImmediately=ChangeTask and the CPM will change the password based on the immediate interval.
    - If the user doesn't release manually, CPM will release the account and change the password once the MinValidityPeriod has passed.
     
    Exclusive & allow manual change (without one-time password):
    - Account is locked when retrieved.
    - If user released manually, we set the account for ResetImmediately=ChangeTask and the CPM will change the password based on the immediate interval.
    - If the user doesn't release manually the account will stay locked.
     
    Exclusive & one-time password (without allow manual change):
    - Account is locked when retrieved.
    - If user released manually, the password won't change.
    - If the user didn't release manually the account will be released in the One-time Password cycle.
     
    Exclusive (without one-time password & allow manual change):
    - Account is locked when retrieved.
    - CPM will never change the password (if you think you are in this mode, but your password changes, you should uncheck AllowManualChange)

    One-time password & allow manual change (without exclusive):
    - Account is NOT locked when retrieved 
    - The password WILL change by MinValidityPeriod because we count the time from the last time it was used (not locked). If the policy is set to periodic change, the password will also change in the periodic cycle.
    - If the policy is set to periodic change, the password will change in the periodic cycle.
     
    One-time password (without exclusive & allow manual change):
    - Account is NOT locked when retrieved.
    - The password will NOT change by MinValidityPeriod because the one-time change requires AllowManualChange to be set to "yes". The account will be found, but ignored (see logs).
    - If the policy is set to periodic change, the password will change in the periodic cycle.
     
    Without exclusive, one-time password & allow manual change:
    - Account is NOT locked when retrieved.
    - The password will NOT change by MinValidityPeriod because both one-time passwords and AllowManualChange are off.
    - If the policy is set to periodic change, the password will change in the periodic cycle.
     
    Notes:

        Any changes to the master policy settings require the refresh interval of the CPM to pass or a restart of the Cyber Ark Password Manager Service
        Also, check that the PasswordManagerUser has "unlock user" permissions.
        The platform Interval setting also has an impact on when CPM will perform password changes. For details, see CPM - Password change time and reset immediately time frame, change now

Product
    Privileged Access Manager (PAM, self-hosted);Privilege Cloud
