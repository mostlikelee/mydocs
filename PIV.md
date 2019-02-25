Smart Card Notes
================

< 10.7 Deprecated
Smart Card Services (framework)
Smart Card CCID (card drivers)
TokenD (service to communicate with card)

Current = CryptoTokenKit API


Hardware (can buy all these from Apple)
---------------------------------------
Onmikey 3121

Mobile Hardware:
Identive Cloud 2900R
Identive SCR3500
Identive SCR3500A

Middleware Drivers available, but PIV is usually supported by the OS since its a US govt standard


Behavior Notes
--------------
Depending on config on smart card, entering invalid PIN x many times may lock the card

Authentication Methods (LEGACY?)
----------------------
Local Account - Issues
- Smartcard PIN *must match* local account password
  - immediate keychain errors as the PIN fails to unlock the "login" keychain
- *requires* a ROOT CA Certificate to validate the card certificate
- any card belonging to any user can be "attached" to any local account
- 

PKINIT
------
Legacy Config (10.6.3 - 10.12?)
`/etc/cacloginconfig.plist`

Current Config (10.12):
`/etc/SmartcardConfig.plist`
Sample:
```
<key>AttributeMapping</key>
<dict>
	<key>fields</key>
	<array>
		<string>Common Name</string>
		<string>RFC 822 Name</string>
	</array>
	<key>formatString</key>
	<string>$2-$1</string>
	<key>dsAttributeString</key>
	<string>dsattrTypeNative:longName</string>
</dict>
```
Note `formatString` which specifies if multiple strings are joined together

Commands
--------

Others:
`security list-smartcards`
`security export-smartcard`
`security smartcards`

associate a user with a public key on a card
`sc_auth pair`
list all identities on all smart cards
`sc_auth identities`
list all public keys associated with a user
`sc_auth list`



Documentation
-------------
`man ssh-keychain`
`man SmartCardServices`
`man SmartCardServices-legacy`
`man pam_smartcard`

Commands (LEGACY)
-----------------
User authentication at the login window and screensaver:
`security authorizationdb smartcard enable|disable`

Display all hashes on the system
`sc_auth hash`
Add the hash of a cert to a user account
`sc_auth accept -u *username* -h *hash*`
Show any certificate hashes for a given username
`sc_auth list -u *username*`



Config Profiles
---------------

Locks Screen when card is pulled out
payloadType: com.apple.screensaver
Key: tokenRemovalAction
Type: Int
Setting: 1|0

Enable|Disable prompt when card is plugged in (does not affect existing pairings)
payLoadType: com.apple.smardcard
Key: UserPairing
Type: Bool

Enable|Disable cards for login/screesaver/auth
Key: allowSmartCard
Type: Bool

Make sure certificates on the card are trusted
Key: checkCertificateTrust
Type: ?

Restrict user account to a single smart card, as opposed to multiple users on 1 account.  Doesn't affect current pairings
Key: OneCardPerUser
Type: Bool

Disable FileVault auto-login
payloadType: com.apple.loginwindow
Key: DisableFDEAutoLogin
Type: Bool

Show list of users at login window (maybe legacy requirement?)
payloadType: com.apple.loginwindow
Key: SHOWFULLNAME
Type: Int (0)

Turn on smartcard debug (com.apple.security.smartcard.log https://ludovicrousseau.blogspot.com/2015/02/debug-smart-card-application-on-yosemite.html)
payloadType: com.apple.security.smartcard
Key: Logging
Type: Bool