<img align="left" src="/logo.png" width="140" height="140" style="margin-right: 10px"/>

AMPEL is a _modular_ and _scalable_ platform with explicit _provenance_ tracking, suited for systematically processing large &mdash; possibly complex and heterogeneous &mdash; datasets either in real time or offline. This includes selecting, analyzing, updating, combining, enriching and reacting to data.

The framework requires analysis and reaction logic to be broken down into adequate independent units.
AMPEL was originally developed to solve challenges in the context of experimental astrophysics, but is general enough to be applicable in various fields.

### Getting started

Want to develop a science program for [ZTF](http://www.ztf.caltech.edu), or adapt AMPEL to a different science case? Have a look at the [core documentation](https://ampelproject.github.io/Ampel-core), start developing your own plugin from the [template repository](https://github.com/AmpelProject/Ampel-contrib-sample), or write to [ampel-info@desy.de](mailto:ampel-info@desy.de).

### Repositories

AMPEL consists of a set of Python packages that can be mixed and matched to cover a target science case without introducing dependencies on packages from other domains.

- [Ampel-interface](https://github.com/AmpelProject/Ampel-interface): base classes for all AMPEL components
- [Ampel-core](https://github.com/AmpelProject/Ampel-core): framework implementation
- [Ampel-photometry](https://github.com/AmpelProject/Ampel-photometry): support for photometric data
- [Ampel-alerts](https://github.com/AmpelProject/Ampel-alerts): support for streaming alert data

### Citation

J. Nordin, V. Brinnel, J. van Santen, M. Bulla, U. Feindt et al. _Transient processing and analysis using AMPEL: alert management, photometry, and evaluation of light curves_. Astronomy and Astrophysics 631 (2019) A147. [doi:10.1051/0004-6361/201935634](https://doi.org/10.1051/0004-6361/201935634)
