+++
title = "Extending Stable Balancing Weights for Hierarchical Data"

# Date first published.
# date = "May 30, 2020"

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Max Rubinstein"]

# Publication type.
# Legend:
# 0 = Uncategorized
# 1 = Conference proceedings
# 2 = Journal
# 3 = Work in progress
# 4 = Technical report
# 5 = Book
# 6 = Book chapter
publication_types = ["3"]

# Publication name and optional abbreviated version.

# Abstract and optional shortened version.
abstract = "Policy analyses frequently conduct analyses where the level of treatment is at some higher unit of aggregation than the data. For example, we might be interested in the average effect of COVID-19 mask mandates -- a state-level policy change -- on county-level mortality rates among counties that received treatment. The goal is then to estimate what would have happened in those counties absent treatment. This paper proposes an extension to Stable Balancing Weights (SBW), proposed by Zubizaretta (2015), for this setting that we call Hierarchical Stable Balancing Weights (H-SBW). We show that our objective function reduces the variance of our estimator relative to SBW when estimating a counterfactual given this hierarchical data struture. We motivate our estimator using model-based notions of variability, linear models for the potential outcome absent treatment, and an unconfoundedness assumption. We demonstrate the performance of our estimator against SBW in simulations calibrated to aggregated American Communities Survey data and show that H-SBW has improved performance."

abstract_short = "Policy analyses frequently conduct analyses where the level of treatment is at some higher unit of aggregation than the data. For example, we might be interested in the average effect of COVID-19 mask mandates -- a state-level policy change -- on county-level mortality rates among counties that received treatment. The goal is then to estimate what would have happened in those counties absent treatment. This paper proposes an extension to Stable Balancing Weights (SBW), proposed by Zubizaretta (2015), for this setting that we call Hierarchical Stable Balancing Weights (H-SBW). We show that our objective function reduces the variance of our estimator relative to SBW when estimating a counterfactual given this hierarchical data struture. We motivate our estimator using model-based notions of variability, linear models for the potential outcome absent treatment, and an unconfoundedness assumption. We demonstrate the performance of our estimator against SBW in both simulations calibrated to aggregated American Communities Survey data and show that H-SBW has improved performance."

# Featured image thumbnail (optional)
image_preview = ""

# Is this a selected publication? (true/false)
selected = true

# Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter the filename (excluding '.md') of your project file in `content/project/`.
#   E.g. `projects = ["deep-learning"]` references `content/project/deep-learning.md`.
projects = []

# Links (optional).
url_pdf = ""
url_preprint = ""
url_code = ""
url_dataset = ""
url_project = ""
url_slides = "https://1drv.ms/b/s!AhALRj1urnKLgd8PJ3XuBxQ6pq9MuQ"
url_video = ""
url_poster = ""
url_source = ""

# Custom links (optional).
#   Uncomment line below to enable. For multiple links, use the form `[{...}, {...}, {...}]`.
# url_custom = [{name = "Recent Presentation", url = "https://1drv.ms/u/s!AhALRj1urnKLgd8RqFI5KFib09sBOw?e=hkoZdS"}]

# Does the content use math formatting?
math = true

# Does the content use source code highlighting?
highlight = true

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = ""
caption = ""

+++
