# Credential Request Flow

The 3 steps in the credential request flow, from the recipient's perspective, are:

1. Initiate Request / Authenticate
2. Form Credential Request
3. Submit Credential Request
4. Receive Credential

## 1. Initiate Request / Authenticate

The goals of step 1 are to ensure the recipient is authenticated, and to navigate the recipient to the "request" page of the FlexID wallet app.

`DEEP_LINK` contains information needed by the issuer and the FlexID wallet app. The issuer provides this link to eligible recipients, typically by email or a link in a dashboard (it may be exposed as a QR code).

a. The user clicks on the link, which opens the wallet with the state it needs to proceed
b. The wallet prompts the user to authenticate (by information provided by the deep link).  FlexID's current implementations use oauth 2.0, but this can be adapted to other flows.

Example of DEEP_LINK:

```
FlexIDrequest://request?                  // FlexID: mobile app deep link
    &auth_type=<auth_type>             // authentication protocol type (e.g. code, saml)
    &issuer=<issuer>                   // Key of the auth configuration
    &vc_request_url=<vc_request_url>   // FlexID: verifiable credential request url
    &challenge=<challenge>             // FlexID: challenge for signing
```

The issuer parameter is used to determine the [OpenID Connect Provider Config](https://openid.net/specs/openid-connect-discovery-1_0.html#ProviderConfig); i.e. information such as:

- authorize_url
- token_url
- client_id

## 2. Form Credential Request

In this step, the wallet forms the signed credential request. This structure follows the Verifiable Presentation format, and we'll refer to it as `REQUEST_PAYLOAD`. This works as follows:

The wallet uses the following input parameters to generate and sign a Verifiable Presentation (using the `sign-and-verify-core` library):

- `challenge`: from `DEEP_LINK`
- `did`: generated by the wallet
- `presentation_id`: optional

The result, `REQUEST_PAYLOAD` has this structure.
```
  {
    "@context": ...
    "type": "VerifiablePresentation",
    "holder": <DID>,
    "proof": {
      "challenge": "<Expected 1-time challenge>",
      "verificationMethod": {
         ...
      }
      ...
    }
  }
```


[See detailed information about the credential request contract](https://github.com/FlexID/sign-and-verify/blob/master/README.md#overview-of-credential-request-flow)

## 3. Submit Credential Request

Lastly, the wallet submits the `CREDENTIAL_REQUEST` to `vc_request_url` using `auth_token` in the headers, as follows

```
curl --header "Content-Type: application/json" \
  --header "Authorization: Bearer <auth_token>" \
  --request POST \
  --data <REQUEST_PAYLOAD> \
   <vc_request_url>
```
   
Note that `vc_request_url` may include additional parameters that the FlexID client app will pass along. This is useful, for example, for specifying additional state needed to lookup the subject's credential to be issued, e.g. [&issuance_id=<issuance_id>...]


![](cred_request_cropped.jpg)


## 4. Receive credential

The issuer's credential request service performs the steps described in "Issue Credential Implementation", and returns the credential(s) to the recipient's wallet.

### Issue Credential Implementation

On the issuer side, the steps are:
1. Check auth state, as needed
2. Verify the subject's `did`
    - Use `sign-and-verify` library/service
    - Endpoint: `/verify/presentations`
    - [See detailed information](https://github.com/FlexID/sign-and-verify/blob/master/README.md#did-proof-of-control-verification)
3. If verified, extract (and store) the subject's DID
4. Lookup/construct credential
    - Use session state to lookup which credential we want to issue to the subject, and construct credential
    - Add the subject DID (from step 3) to the credential (`credentialSubject.id`)
5. Sign (issue) the credential 
    - Use `sign-and-verify` library/service
    - Endpoint: `/issue/credentials`
6. Store credential and related accounting information
7. Return the VC

