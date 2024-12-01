
Microsoft Entra ID features to help mitigate risk of unauthorized access:
- **Password complexity rules**
- **Password expiration rules**
- **Self-service password reset (SSPR)**
- **Microsoft Entra ID Protection**
- **Microsoft Entra password protection**
- **Microsoft Entra smart lockout**
- **Microsoft Entra application proxy**
- **Single sign-on (SSO)**
- **Microsoft Entra Connect**


## Microsoft Entra multifactor authentication

Supplies added security for your identities by requiring two or more elements for full authentication.

1. **Something you know**
2. **Something you have**
3. **Something you are**

## Supported authentication methods

1. **Mobile App Verification code**
2. **Mobile app notification**
3. **Call to a phone**
4. **FIDO2 security key**
5. **Windows Hello for Business**
6. **OATH tokens**

## Configure multifactor authentication options

Entra admin -> Identity -> Protection -> Multifactor authentication -> Configure (Additional cloud-based multifactor authentication settings)

# Configure multifactor authentication methods

|Authentication method|Services|
|---|---|
|**Password**|Microsoft Entra multifactor authentication and SSPR|
|**Security questions**|SSPR|
|**Email address**|SSPR|
|**Windows Hello for Business**|Microsoft Entra multifactor authentication and SSPR|
|**FIDO2 Security Key**|Microsoft Entra multifactor authentication and SSPR|
|**Microsoft Authenticator app**|Microsoft Entra multifactor authentication and SSPR|
|**OATH hardware token**|Microsoft Entra multifactor authentication and SSPR|
|**OATH software token**|Microsoft Entra multifactor authentication and SSPR|
|**Text message**|Microsoft Entra multifactor authentication and SSPR|
|**Voice call**|Microsoft Entra multifactor authentication and SSPR|
|**App passwords**|Microsoft Entra multifactor authentication in certain cases|
## Monitoring adoption

Entra admin -> Identity -> Protection -> Authentication methods -> Activity



