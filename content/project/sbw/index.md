+++
# Project title.
title = "Stable Balancing Weights for Group-Level Data"

# Date this page was created.
date = 2019-12-02T00:00:00

# Project summary to display on homepage.
summary = "We propose a modification of the Stable Balancing Weights (SBW) objective function proposed by Zubizaretta (2015) to accomodate group-level error components.
We show that this estimator has improved efficiency relative to SBW in both simulations and real-world settings."

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["cpe"]

external_link = ""

# Links (optional).
url_pdf = ""
url_slides = ""
url_video = ""
url_code = ""

# Custom links (optional).
#   Uncomment line below to enable. For multiple links, use the form `[{...}, {...}, {...}]`.
# url_custom = [{icon_pack = "fab", icon="r-project", name="R Package", url = "https://github.com/Mr8ND/TC-prediction-bands#2-tcpredictionbands-package"}]


# Featured image
# To use, add an image named `featured.jpg/png` to your project's folder. 
[image]
  # Caption (optional)
  caption = ""

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = "Smart"
+++

We propose an alternative to the Stable Balancing Weights (SBW) that seeks to minimize the mean square error (MSE) 
of the estimated missing counterfactual when using group-level data. 
Specifically, we modify the SBW objective function, proposed by Zubizaretta (2015), 
to account for group-level data variance structures (SBW-G), and outline a set of assumptions under which optimizing 
this objective will return the set of minimum variance unbiased convex weights. We then highlight a plausible 
individual-level data generating process (DGP) that could potentially bias estimated treatment effects when using 
aggregated survey data that appears to not have been previously emphasized. We show both that SBW-G will return 
weights within the constraints that minimize the MSE under this DGP, and that we can alternatively easily return the bias minimizing weights. 
Similar to Abadie et al (2015), we use pre-treatment data for model selection, and demonstrate the efficacy 
of our method by comparing the performance against SBW and against weighted least squares (WLS) 
using simulated outcomes from the American Communities Survey (ACS). We conclude by demonstrating 
the use of our method to estimate the effect of Medicaid expansion on uninsurance rates 
in 2014 among states that expanded Medicaid.
