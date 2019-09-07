---
layout: post
title: A long and overdue PyICA update
subtitle: Rebooting the PyICA project
---

After a few months away from the PyICA project, an itch to finish what I had started set in. Looking back at my old
code, a few issues became apparent. The fist and most daunting: GUI creation for Python based apps is never easy.
GUI builder such as wxGlade (for wxPython, the original GUI library considered for this project), the integration into
the final project is still quite the difficult task.

The second less apparent but still present issue, is how to control the users ability to refresh market data. Currently,
PyICA polls for data directly from the CCP provided endpoints for EvE. This creates an issue, as some of the queries can
sometimes create errors. It had been documented in un-official capacities in the past that generating an error on an
endpoint too many times in a short span of time will lead to CCP banning the app altogether from interacting with its
servers. Putting two and two together, having an "Update" button in the PyICA UI allowing the users to update their data
as they please could lead to dire consequences. Various means of controlling user updates such as timing restrictions
were considered but never fully implemented due to the possibility of being easily circumvented.

A solution was established that nailed both birds with one stone: converting PyICA from an offline Python client, to a
web-based Python app. Porting the already existing backend (with some tweaks) to a Python web-framework such a Django
can help both create the UI in a much more intuitive way, keep users updated to new releases and updates to PyICA, and
keeping data polling from CCP servers to a minimum by keeping a cache of market data on the web server.

This is a drastic change to the original vision of PyICA, but hopefully allows it to one day become a fully functional 
product. Come back next week for another update on how the project is doing.