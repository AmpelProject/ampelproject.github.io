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
