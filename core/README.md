Intro here...

# Database

MongoDB is used to store data.
The collections have been designed and indexed for fast insertion and query.
Users do not interact with the database directly.
Information exchange is instead regulated through (python) abstract base classes from which units are constructed.
A specific set of internal classes handle database input and output.


# Containers

All AMPEL software, can be combined into one container that defines an instance.
These containers can be used both to process real-time data as well as to reprocess archived data.
The containers themselves should be archived as well.

<!--
Astronomers have during the past century continuously refined tools for
analyzing individual astronomical transients. Simultaneously, progress in instrument and CCD
manufacturing as well as new data processing capabilities have led to a new generation of transient
surveys that can repeatedly scan large volumes of the Universe. With thousands of potential candidates
available, scientists are faced with a new kind of questions: Which transient should I focus on?
What were those things that I dit not look at? Can I have them all?

Ampel is a software framework meant to assist in answering such questions.
In short, Ampel assists in the the transition from studies of individual objects
(based on more or less random selection) to systematically selected samples.
Our design goals are to find a system where past experience (i.e. existing algorithms and code) can consistently be applied to large samples, and with built-in tools for controlling sample selection.
-->
