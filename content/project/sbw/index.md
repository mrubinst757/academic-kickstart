+++
# Project title.
title = "Stable Balancing Weights for Group-Level Data"

# Date this page was created.
date = 2019-12-02T00:00:00

# Project summary to display on homepage.
summary = "We modify the Stable Balancing Weights objective function proposed by Zubizaretta (2015) to account for group-level panel-data. In particular, we propose modifications that return the minimum bias and minimum mean-square-error when the covariates and outcome are measured with sampling variability. We also propose a modification that allows for dependence between regions when treatment assignment is at a higher level that the units. We then show how we can tune this algorithm using pre-treatment outcomes. This paper also relates to the synthetic controls literature, but restricts attention to the case where we can exactly balance the covariates and where we have multiple treated and control units."

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
We propose a modification of the Stable Balancing Weights (SBW) objective function (Zubizaretta (2015)) for use with region-level data to accomodate sampling variability in the region-level covariates and outcomes. We show that this estimator has improved efficiency relative to SBW in both simulations and real-world applications.
