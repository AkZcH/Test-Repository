# Introduction
In engineering, reliability is the probability or duration that a product/service/system will perform its intended function failure-free, under stated under stated conditions for a specified interval.
## Components of Reliability-
1. Dependibility
2. Availibility
3. Durability
4. Probability
5. Quality over time
## Why to design for reliability
- Customer statisfaction
- Brand Value & Reputation
- Lower lifecycle costs
- Lower warranty costs
- Safety and Compliance
## Reliability during different phases
#### Concept Phase:
Requirements specifications, safe planning
#### Design:
FMEA, adopting fault-tolerant design
#### Manufacturing:
Manufacturing process capability, quality control
#### Operations:
Maintenance Strategy
Failure Analysis and Mitigation
## Design Techniques for Reliability
#### Redundancy:
Replication of important components and services in the system, in case one fails, so the replicas can take over. e.g- Having DB replicas all around the world
#### Derating:
Operation of electrical and electrical components below their maximum rated capacity for safety and prevention of load
#### Stress-Strength Analysis:
Assessing the probability of failure of a component by comparing its applied stress to its strength
#### Modular Design:
Having smaller, independent working modules, with a specific function to perform. This prevents the failure of one components bringing the whole system down.
#### Robust Design:
Manufacturing of products with robust build and quality, tested repeatedly to ensure reliability

---
# Reliability Calculation
# $$
\text{R}(t) = e^{-\lambda.t}
$$
R(t) - Reliability for time interval 't'
e - natural log number (2.7182)
Lambda - Failure Rate
t = Time Interval given

$$
	\text{Failure Rate}(\lambda) = \frac{No. of Failures}{Total Operating Time}
$$

## Reliability for cluster of components

#### i. For Components in Series
# $$
	\text{R}(t) = \prod_{i}^{n} {R_i}
$$
The {R_i} is to be converted to decimals if given in percentages

#### ii. For Components in Parallel

# $$
\text{R}(t) = 1 - \prod_{i}^{n}(1 - R_i)
$$
---
# Reliability vs. Quality

| Reliability                                               | Quality                                      |
| --------------------------------------------------------- | -------------------------------------------- |
| Measured at the time of Purchase                          | Measured over a period of time               |
| Checks to see if it meets specifications and expectations | Checks to see if it performs without failure |
| Focusses on initial performance and features              | Focusses on long-term dependibility          |
### Tests for Reliability
- #### Accelerated Life Testing
- #### Highly Accelerated Life Testing
- #### Burn in Testing
- #### Environmental Stress Screening
## Reliability Measures
#### Mean Time To Failure (MTTF):
The average life-cycle of a product from manufacturing to being beyond repair
#### Mean Time Between Failure (MTBF):
The average time duration of a product between 2 consecutive failure, being within the repair scope.