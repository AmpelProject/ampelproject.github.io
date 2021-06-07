---
layout: default
title: ZTF
has_children: true
nav_order: 1
---

# Services

The live Ampel instance that processes the ZTF alert stream at DESY-Zeuthen relies on a few auxiliary services. Some of these are provided for limited public use, e.g. for testing contributed Ampel units.

## Catalog matching

The [catalog matching service](https://github.com/AmpelProject/catalog-server) provides cone searches against [extcats](https://github.com/AmpelProject/extcats) and [catsHTM](https://github.com/maayane/catsHTM) catalogs via a REST API. You can try it out via the [online documentation](https://ampel.zeuthen.desy.de/api/catalogmatch/docs).

## ZTF alert archive

The DESY alert archive contains ZTF alert packets issued since June 2018. Alerts from the last ~3 months include cutout images; older alerts contain only the candidate photometry and history. The API is detailed in the [online documentation](https://ampel.zeuthen.desy.de/api/ztf/archive/v2/docs).

## Live database query

The [live query API](https://ampel.zeuthen.desy.de/api/live/docs) allows you to query stocks (astronomical transients) that are currently being tracked by the Ampel "live" instance at DESY. This API is experimental and subject to change at any time.

To use this API, you must join the [AmpelProject organization](https://github.com/AmpelProject), and visit the [Ampel dashboard](https://ampel.zeuthen.desy.de/live/dashboard/tokens) in a web browser to retrieve an access token. This may ask you to authorize the `ampel-auth-test` app to read your organization memberships. Once authorized, you should see content like this: 

<img width="886" alt="dashboard_token" src="https://user-images.githubusercontent.com/938705/117054597-8cec8d80-ad1a-11eb-957e-82eccb47496c.png">

You can use the generated API token like so:

```
> curl -H "Authorization: bearer $TOKEN" -sL https://ampel.zeuthen.desy.de/api/live/stock/ztf/ZTF21aaqhhfu | jq
{
  "_id": 119032484,
  "channel": [
    "RCF_2020B",
    "HU_TNS_MSIP",
    "WEIZMANN_INFANTSN",
    "HU_TNS_PARTNER"
  ],
  "created": {
    "HU_TNS_MSIP": "2021-03-20T08:14:13",
    "RCF_2020B": "2021-03-20T08:14:13",
    "any": "2021-03-20T08:14:13",
    "HU_TNS_PARTNER": "2021-03-20T09:06:46",
    "WEIZMANN_INFANTSN": "2021-03-20T09:06:46"
  },
  ...
}
```

where `TOKEN` is the token you generated above. The token will expire after 24 hours; you may visit the dashboard at any time to get a new token valid for another 24 hours.


