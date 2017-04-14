:tocdepth: 1

.. Please do not modify tocdepth; will be fixed when a new Sphinx theme is shipped.

.. sectnum::

.. Add content below. Do not include the document title.

Minimoon detections with LSST - an investigation with MAF and MOPS.
===================================================================

This technote is a short writeup of an investigation of LSST's
capabilities to detect minimoons -- temporarily captured orbiting
(TCO) or flyby (TCF) satellites. For more background on temporarily
captured natural earth-orbiting satellites such as TCOs and TCFs,
please see [Granvik2012]_ and [Fedorets2017]_.

Orbital distributions for a population of TCOs and TCFs were provided
by Grigori Fedorets. These populations contain objects which go
through temporary capture over a Metonic cycle (approximately 19
years, starting in 2008 and going through 2027). As the population is
almost entirely composed of very small objects, the only times they
are bright enough to be visible is during their capture.

After an unsuccessful attempt to generate observations "as seen by an
OpSim simulated survey (minion_1016)" [#label]_, Fedorets provided a
series of ephemerides for each object .. these were sets of 4
observations corresponding to 2x15s snaps in two visits spaced 30
minutes apart, on every night where the object had a V magnitude
<24.5.  While not truly realistic, this does represent the best possible
upper limit of what LSST could do and does give us some indication of
what limits MOPS may have in being able to link the minimoons. It also
provides a way to accomodate the fact that the simulated LSST survey
covers the time period 2022-2032 (when LSST is expected to be on the
sky) but that the capture times for these particular TCO and TCF
populations cover 2008-2027, by just using a subset of these idealized
observations from the period MJD = 57755 to 61403 (2017 to 2026).

A color term (to translate to LSST *r* band) and trailing losses were
added to the ephemerides provided by Fedorets. To determine the color
term, each object was simply assumed to have an S type spectra, which
was then combined with the LSST *r* bandpass to calculate *V-R* =
-0.26. To calculate trailing losses, a seeing value of 0.7" was
assumed; with the velocity at each observation, trailing losses can be
calculated as explained in this notebook on `Trailing Losses`_ (and
illustrated in fig-trailing-losses_). For the fast-moving TCOs and
TCFs (see fig-velocity-tco_ and fig-velocity-tcf_, the trailing losses
are significant.

The resulting simulated observations (a subset of all possible
ephemerides, where *V*<24.5 and covering a ten year period) were then
evaluated for discovery possibility and length of observational arc
using MAF, and all observations with SNR>5 were run through MOPS.

.. _Trailing Losses: https://github.com/lsst-pst/neo_capabilities/blob/master/Trailing%20Losses.ipynb


.. figure:: /_static/trailing_losses.png
   :name: fig-trailing-losses
   :target: ../../_static/trailing_losses.png

   Trailing losses in 0.7" seeing. The blue dotted line (SNR loss)
   indicates the losses due to simply spreading the light from a
   moving source over more background pixels. The red line (Detection
   loss) indicates the losses due to detection algorithms assuming a
   stellar PSF instead of a trailed PSF. With additional work in the
   source detection software stage, detection losses can be mitigated
   to the level of SNR losses. For the purposes of this work, we assumed
   the SNR losses as the appropriate trailing losses.


.. figure:: /_static/velocity_tco.png
   :name: fig-velocity-tco
   :target: ../../_static/velocity_tco.png

   Velocity distribution for all V<24.5 observations of TCOs over a ten year
   period. The observations of objects which will be determined as 'findable' by MAF
   are in the red histogram; observations of all objects are combined
   in the yellow histogram.


.. figure:: /_static/velocity_tcf.png
   :name: fig-velocity-tcf
   :target: ../../_static/velocity_tcf.png

   Velocity distribution for all V<24.5 observations of TCFs over a ten year
   period. The observations of objects which will be determined as 'findable' by MAF
   are in the red histogram; observations of all objects are combined
   in the yellow histogram.



.. [#label] Creating simulated LSST opsim-based observations of
            minimoons was challenging due to several factors. Because
            we wanted to run the resulting observations through MOPS,
            the positional accuracy from a simple linear interpolation
            for these fast-moving objects was not
            sufficient. Generating ephemerides through chebyshev
            polynomials or even direct use of OpenOrb is possible, but
            would be extremely slow. Unfortunately, development of
            smarter code to do this kind of work for us is still
            underway.



MAF Results
===========

We used MAF to look for series of observations which could potentially
result in discovery linkages -- specifically, we looked for sets of
observations where the object was brighter than SNR=5 on 3 separate
nights inside of a 15 night window, where in each night the
observations were separated by at least 15 minutes and less than 90
minutes.


MOPS Results
============

We created inputs for MOPS from all observations where the SNR was
greater than 5 (assuming a 5-sigma limiting magnitude of *r*=24.5 for all
observations).



References
==========

.. [Granvik2012]  Granvik, Vaubaillonc, & Jedicke 2012. *The
                  population of natural Earth satellites.*
                  `<http://www.sciencedirect.com/science/article/pii/S0019103511004684>`_

.. [Fedorets2017]  Fedorets, Granvik & Jedicke 2017. *Orbit and size distributions for asteroids
                  temporarily captured by the Earth-Moon system.*
                  `<http://www.sciencedirect.com/science/article/pii/S0019103516306480>`_


.. note::

   **This technote is not yet published.**

   Minimoon detections in LSST: MAF and MOPS investigation
