---
title: "Scope"
description: "This page summarizes how Oauth2 scopes are used in ID-porten OIDC provider."
summary: 'This page summarizes how Oauth2 scopes are used in ID-porten OIDC provider.'
permalink: oidc_protocol_scope.html
sidebar: oidc
product: ID-porten
---


ID-porten can issue tokens to scopes controlled by Difi, as well as scopes controlled by other organizations.

## Scopes controlled by third parties

Such scopes will always follow this syntax:

```
scope ::= prefix ':' subscope
```

The `prefix` is a string which is manually linked to a specific organization.  It may be the organization name, or other suitable value.  An organization may have multiple prefixes.

The `subscope` is created by the owning organization itself using selfservice.  ID-porten place no specific rules on how subscopes should be named or structured, as different organizations have vastly different needs to structure their APIs. Nevertheless, some _recommendations_ apply:

- the prefix should natually identify the owning organization or its subsidiary / application (example `nav` or  `folkeregisteret` or organization number)
    - if multiple organizations share the same scope, the prefix should identify the sector (`forsikring`)
- subscope should primarily identify the resource and not the API (`trygdeopplysninger` or `adresse`)
- subscope should contain various postfixes to differentiate between read and write access to the resource (`nav:trygdeopplysninger.write`)
     - absence of a postfix should normally only imply read access

For access to these scopes, you need to contact the organization owning the scope.

| URL to list of scopes|Description|
|-|-|
| [https://integrasjon.difi.no/scopes/all](https://integrasjon.difi.no/scopes/all)  | A list of scopes protected by ID-porten in Production |
| [https://integrasjon-ver2.difi.no/scopes/all](https://integrasjon-ver2.difi.no/scopes/all)     |  A list of scopes protected by ID-porten in VER2 environment.  |


## Reserved scopes

The following scopes triggers special treatment in ID-porten OIDC provider.  They can be used by all customers.

|Scope|Description|
|-|-|
|openid   | Triggers an OpenID Connect-compliant authentication  |
|profile  | Gives access to the /userinfo endpoint   |
|no_pid   | Triggers a [pseudonymous authentication](oidc_func_nopid.html)   |
|eidas    | Include the eIDAS attributes in the id_token   |

## Scopes for APIs from Digitaliseringsdirektoratet 

You need to ask us for permission to be able to use these scopes:

| Scope |Description|
|-|-|
|idporten:dcr*  | Scopes allowing for self-service of ID-porten integrations   |
|idporten:scopes*   | Scopes allowing for self-service of ID-porten/Maskinporten API management    |
|global/*    | Scopes for global access to the Contact Registry |
|user/*      | Scopes giving Contact Registry details for the authenticated users  |

## Scope limitations

If the attribute allowed_integration_types isn't empty, the scope is limited to that usage. The current integration types are:

| Integration Type |Description|
|-|-|
|idporten   | requires end-user authentication   |
|maskinporten  | only for server to server integration   |
|krr   | for kundereservasjonsregister   |
|eformidling    | for eformidling  |
|api_klient    | requires end-user authentication. DigDir service.  |

You will not be able to add a scope to your client if there is a conflict with the client's integration type and the scope. E.g. you can't add a "maskinporten" scope to a "idporten" client.
