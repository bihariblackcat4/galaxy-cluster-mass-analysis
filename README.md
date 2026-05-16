# Galaxy Cluster Mass Estimation & Luminous-to-Dynamical Mass Discrepancy

An astrophysics data pipeline utilizing Python and observational data from the Sloan Digital Sky Survey (SDSS) to calculate and compare the **Dynamical Mass** (via velocity dispersion) and **Luminous Mass** (via r-band magnitude integrations) of a galaxy cluster. 

## Overview

This project implements standard observational cosmology techniques to investigate the presence of dark matter within an observationally identified galaxy cluster. By isolating cluster members through redshift filtering ($3\sigma$ clipping), the script transforms spectroscopic data into line-of-sight velocities within the cluster frame, computes the system's dynamical boundaries, and directly evaluates the classical mass-to-light variance.

### Key Technical Implementations:
* **Data Aggregation:** Handled SDSS SQL data grouping by unique object IDs (`objid`) to calculate mean spectroscopic redshifts (`specz`).
* **Outlier Rejection:** Utilized standard statistical filtering to remove foreground/background field galaxies.
* **Cosmological Calculations:** Implemented $H_0$ conversions, comoving distances, and angular diameter conversions using `astropy.constants` and `astropy.cosmology`.
* **Mass Metrics Evaluation:** Calculated Virial-based dynamical mass $M_{\text{dyn}}$ alongside integrated $r$-band total luminosities for light-derived mass estimates.

---

## Methodology & Physics Formulation

### 1. Velocity Dispersion & Cluster Frame Transformation
Galaxies are filtered within a $\pm 3\sigma$ redshift window around the mean cluster redshift $\bar{z}$. To evaluate the dynamics, redshifts are converted to line-of-sight velocities relative to the cluster's rest frame using:

$$v = c \cdot \frac{z - \bar{z}}{1 + \bar{z}}$$

The standard deviation of these velocities yields the **Velocity Dispersion ($\sigma_v$)**, which for this cluster was determined to be:
$$\sigma_v \approx 1,218,583.41 \text{ m/s} \quad (\approx 1218.6 \text{ km/s})$$

### 2. Dynamical Mass Estimation ($M_{\text{dyn}}$)
Using the Virial Theorem approximation for a spherical system, the total mass enclosed within the calculated cluster diameter ($R = \text{Diameter}/2$) is expressed as:

$$M_{\text{dyn}} = \frac{3 \cdot \sigma_v^2 \cdot R}{G}$$

* **Estimated Physical Diameter:** $1.76 \times 10^{22} \text{ meters}$
* **Calculated Dynamical Mass:** $2.93 \times 10^{14} \ M_{\odot}$

### 3. Luminous Mass Estimation ($M_{\text{luminous}}$)
The $r$-band magnitudes of verified cluster members are converted to absolute magnitudes ($M_r$) using the `Planck18` luminosity distance $D_L$:

$$\mu = 5 \log_{10}(D_L) + 25$$
$$M_r = r_{\text{mag}} - \mu$$

The total luminosity is integrated by evaluating individual contributions against the solar $r$-band absolute magnitude ($M_{\odot, r} = 4.65$):

$$L_{\text{total}} = \sum 10^{\frac{M_{\odot, r} - M_r}{2.5}}$$

Assuming a standard cluster mass-to-light ratio ($\Upsilon \approx 100$), the luminous mass is estimated via:
$$M_{\text{luminous}} = L_{\text{total}} \cdot \Upsilon \approx 2.31 \times 10^{14} \ M_{\odot}$$

---

## Key Results & Insights

| Metric | Computed Value |
| :--- | :--- |
| **Velocity Dispersion ($\sigma_v$)** | $1218.58 \text{ km/s}$ |
| **Cluster Diameter** | $1.76 \times 10^{22} \text{ m}$ |
| **Dynamical Mass ($M_{\text{dyn}}$)** | $2.93 \times 10^{14} \ M_{\odot}$ |
| **Total $r$-band Luminosity** | $2.31 \times 10^{12} \ L_{\odot}$ |
| **Luminous Mass ($M_{\text{luminous}}$)** | $2.31 \times 10^{14} \ M_{\odot}$ |
| **Mass Ratio ($M_{\text{dyn}} / M_{\text{luminous}}$)** | **$1.27$** |

### Data Visualizations:
The analysis includes complete distributions plotted via `matplotlib`:
1.  **Redshift Distribution Histogram:** Shows the distribution spike around the cluster's core velocity.
2.  **Galaxy Velocities in Cluster Frame:** A visual of the velocity dispersion, mapping out the classic Gaussian-like kinematic spread typical of relaxed clusters.

---
