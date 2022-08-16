# High-Channel-Count Thresholding Plugin

## Overview

This is a thresholding plugin for Open Ephys intended to work with very high
channel count setups (thousands of channels).


Operations offered include:

* Optional DC offset removal (zero-averaging).

* Thresholding based on amplitude or on the first difference (difference
between successive samples, used as a proxy of derivative).

* Thresholding against a symmetrical fixed threshold, against different
high and low fixed thresholds, or against symmetrical or asymmetric standard
deviation thresholds.

* Optional hysteresis in the threshold output to avoid glitches and drop-outs.

* Optional averaging of the output over non-overlapping time windows
(reporting the fraction of samples that are above-threshold within each
window).

* Output takes the form of floating-point values in the range 0..1.


Limitations:

* To keep memory footprint low, non-overlapping windows are used for
averaging instead of sliding windows. This causes output discontinuities at
window boundaries (for both the output window and the window used for DC
offset removal and standard deviation calculation).

* Internal calculations use variance rather than standard deviation, to avoid
square roots.

* I'm told that the Open Ephys architecture really doesn't like enormous
numbers of boolean events, so output is kept as floating-point signal series.
This also makes averaging practical, per above.


Typical applications:

* Artifact detection can be performed by setting amplitude and first
difference thresholds in the range of 4-6 sigma for the wideband signal.
Electrical artifacts in particular tend to cause stepwise jumps in signals,
which show up as first difference outliers.

* Spike detection can be performed via amplitude or first difference
thresholding on a high-pass-filtered signal. This does not replace more
sophisticated spike detection algorithms; this is intended for visual or
automated identification of spiking channels so that a subset of channels
can be forwarded to more processing-intensive parts of the signal chain.

* LFP oscillation detection can be performed via amplitude thresholding on
a band-pass-filtered signal using typical oscillation detection thresholds
(2-3 sigma). This is intended for visual or automated identification of
bursty channels so that a subset of channels can be forwarded to more
processing-intensive parts of the signal chain.


Future improvements:

* Adding support for IIR low-pass filtering of the DC level and the
variance might be a good idea, as a lightweight alternative to a sliding
window that avoids the non-overlapping window's discontinuities.


_(This is the end of the file.)_
