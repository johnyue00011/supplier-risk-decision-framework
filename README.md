Executive Summary

This project presents a decision-oriented supplier risk framework for manufacturing nonconformance management.
Using structurally representative synthetic data, it demonstrates how real-time NCR signals can be translated into risk prioritization, escalation thresholds, and operational actions under capacity constraints.

The system models supplier risk as a continuum of severity, distinguishing isolated incidents from recurring patterns that require intervention, such as purge and inventory quarantine.
The focus is on interpretable, exposure-aware signals and tiered decision logic, rather than purely predictive accuracy.


1.

What decision this system supports

    This system supports cross-functional supplier risk prioritization decisions in a manufacturing nonconformance context.
	    •	Decision owners:
            Supplier risk prioritization is a joint decision process involving Quality Engineering, MRB, Procurement, and supplier representatives.
	    •	Quality Engineering and MRB lead risk assessment and technical disposition
	    •	Procurement evaluates supplier impact and corrective action feasibility
	    •	Final prioritization is aligned through cross-functional review rather than a single owner
	    •	Decision frequency: Monthly, with rolling reassessment as new NCRs occur
	    •	Decisions and actions enabled by the risk score:
	        1.	Prioritize suppliers for audit and corrective action reviews
	        2.	Expedite MRB review for suppliers flagged as high risk
	        3.	Apply temporary release holds or tightened incoming inspection policies
	        4.	Allocate limited quality, inspection, and remediation resources under capacity constraints
	    •	Cost of being wrong:
	    •	False negatives (underestimating risk): Production delays, line downtime, scrap, rework, and downstream customer impact
	    •	False positives (overestimating risk): Excessive inspections, slower material flow, increased operational cost, and inefficient resource allocation


2.

    Features (Decision-relevant signals)

    Features are designed to be decision-relevant and interpretable, rather than purely predictive.
    They are selected to support cross-functional alignment across Quality Engineering, MRB, and Procurement.

        1. Exposure-normalized defect signals
	        •	Defect and nonconformance signals normalized by verified incoming material quantity, recorded at receipt after reconciliation with supplier documentation
	        •	Aggregation performed at the supplier level over a rolling time window to account for heterogeneous exposure across suppliers
	        •	Batch identifiers are retained for traceability but not used as the primary exposure denominator


        2. Temporal behavior and recurrence patterns
	        NCR events are created immediately when materials are rejected or questioned by the production team, and are escalated to MRB for disposition.
            As a result, individual NCRs represent real-time weak risk signals rather than delayed administrative records.

            Temporal and recurrence features are constructed to capture escalating risk intensity within this workflow:
	        •	Single-event signals:
                    Isolated NCR occurrences are treated as early indicators of potential supplier risk and contribute to baseline risk prioritization.
	        •	Short-horizon recurrence signals:
                    Repeated NCRs of similar type for the same supplier within a short time window indicate elevated and potentially systemic risk.
            These patterns correspond to operational escalation mechanisms, including purge actions initiated by logistics and MRB.

            In practice, these features allow the system to distinguish one-off anomalies from deteriorating supplier quality, and to support both ranking-based monitoring and threshold-based intervention.

        3. Disposition and resolution outcomes
	        •	Distribution of NCR dispositions (e.g., use-as-is, rework, scrap) as proxies for downstream operational impact
	        •	Escalation-related signals reflecting MRB involvement and resolution complexity

        4. Missingness-as-signal features
	        •	Explicit indicators for missing or delayed reporting fields
	        •	Missingness treated as informative rather than MCAR, reflecting process gaps or governance risk

        5. Stability-oriented aggregation features
	        •	Aggregations at the supplier level designed to reduce sensitivity to single extreme events
	        •	Feature definitions prioritized for temporal stability and interpretability over short-term accuracy gains

    Feature selection rationale
	    •	Features are chosen to align with how risk decisions are reviewed and justified in practice
	    •	Highly complex or opaque features were intentionally excluded to preserve trust and auditability
	    •	The objective is robust prioritization under uncertainty, not point-in-time defect prediction


Decision thresholds & actions

    Risk signals are translated into operational actions through a tiered decision framework, rather than a single cutoff.
    Thresholds are designed to balance early risk visibility with the cost of high-impact interventions.
	    •	Baseline monitoring (low-intensity risk)
	    •	Condition: Isolated NCR events without short-term recurrence
	    •	Action:
	    •	Supplier remains in routine monitoring and ranking queues
	    •	NCRs are handled through standard MRB disposition workflows
	    •	Purpose: Maintain visibility without disrupting normal material flow
	    •	Heightened attention threshold (medium-intensity risk)
	    •	Condition: Repeated NCRs of similar type within a short time window (e.g., 2–3 occurrences within 30 days)
	    •	Action:
	        •	Cross-functional review involving Quality Engineering, MRB, and Procurement
	        •	Increased scrutiny of incoming materials and corrective action progress
	    •	Purpose: Intervene early before systemic risk propagates
	    •	Escalation threshold (high-intensity risk)
	    •	Condition: Persistent or dense recurrence patterns indicating systemic supplier issues
	    •	Action:
	    •	Initiation of purge procedures by Logistics based on MRB-defined criteria
	    •	Full inventory review, quarantine of potentially affected materials, and supplier-side investigation
	    •	Determination of remediation actions, including rework, return, or replacement
	    •	Purpose: Contain high-impact risk and prevent downstream production or customer impact

    This tiered structure ensures that high-cost actions are reserved for sustained risk signals, while allowing early-stage indicators to inform prioritization and monitoring decisions.

3.

    Risk Definition

    Supplier risk is defined as the likelihood of future high-impact nonconformance events, conditional on observed operational signals and verified exposure.
    The objective is to identify suppliers whose recent behavior indicates an elevated probability of escalation, rather than to explain past defects retrospectively.
	    •	Prediction target:
    A forward-looking supplier risk ranking score, representing the relative likelihood of entering a higher-risk operational state in an upcoming time window.
    The score is used for prioritization and tiered decision-making rather than binary classification.
	    •	Impact proxy:
    Risk impact is approximated using a combination of severity-weighted NCR signals and downstream operational consequences, including rework, scrap, and delay proxies.
    These proxies are selected to align with actions that carry material operational cost, such as purge and inventory quarantine.
	    •	Why this is a risk problem (not simple defect monitoring):
	    •	Suppliers exhibit heterogeneous exposure levels, making raw defect counts misleading
	    •	NCR occurrences are not independent events and often cluster temporally
	    •	Missing and delayed information reflects process and governance risk rather than random noise
	    •	Decisions require explicit prioritization and escalation under limited quality and operational capacity
	    •	Output:
            A calibrated risk score that supports ranking, monitoring, and escalation thresholds, enabling proactive intervention before systemic supplier risk propagates into production or customer impact.