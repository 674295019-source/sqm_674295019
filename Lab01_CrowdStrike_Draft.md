# Lab Report 1 (Draft)

## Title

Analysis of the CrowdStrike Falcon Sensor Content Update Incident: Global Windows Outage on 19 July 2024

**Course:** 4663335 Software Quality Management  
**Student ID:** 674295019  
**Name:** [Enter your first and last name]  
**Date:** [Enter submission date]

> Note: This is a working draft. Check the Lab 1 worksheet and rubric in MS Teams before converting it to the final PDF.

## 1. Real software failure case: event and impact

On 19 July 2024 at 04:09 UTC, CrowdStrike released a Rapid Response Content configuration update for the Windows Falcon Sensor. The update was intended to collect telemetry relating to possible new threat techniques. However, affected Windows hosts received problematic content and crashed with a Blue Screen of Death (BSOD). The incident affected Windows hosts running Falcon Sensor version 7.11 or later that were online and received the update between 04:09 and 05:27 UTC. CrowdStrike reverted the defective content at 05:27 UTC. Mac and Linux hosts were not affected.

The incident was serious because the Falcon Sensor is deployed on endpoint computers in many organizations. A computer crash prevents staff from using normal business systems and can interrupt essential services that depend on those computers. The incident therefore illustrates how a defect in a widely distributed software update can propagate quickly and create operational disruption across many organizations.

## 2. Quality characteristics involved

| Quality characteristic | Evidence from the incident | Why it matters |
|---|---|---|
| Reliability | The content update caused an unexpected exception and a Windows system crash. | Software should perform its required functions without failing under specified conditions. |
| Availability | Affected computers could not remain operational after receiving the update. | Users could not access services or perform work while devices were unavailable. |
| Compatibility | The update supplied 21 input fields while the sensor expected 20. | Components exchanging configuration data must correctly handle agreed data formats. |
| Security / resilience | The update came from security software, yet its failure disabled the protected endpoints. | A security product must preserve system stability while it protects the system. |
| Recoverability | Recovery required removal or reversal of the defective content before devices could return to service. | Systems need a safe, timely recovery and rollback mechanism after a failed deployment. |

## 3. Where the problem should have been prevented or detected in the SDLC

### Prevention

1. **Requirements and design:** Define and enforce an explicit data-contract rule between the content configuration and the sensor. The number, type, and allowed range of input fields should be validated before release.
2. **Implementation:** Add bounds checking and defensive error handling so malformed or unexpected content cannot cause a kernel-level system crash.
3. **Testing:** Use automated schema validation, unit and integration tests for boundary cases, fuzzing, fault injection, and rollback tests. A test case using an unexpected field count should fail before production.
4. **Release planning:** Assess the risk of releasing configuration content to a large fleet and require staged deployment plus a tested rollback plan.

### Detection before widespread impact

1. Publish first to a small canary group instead of all eligible computers.
2. Monitor crash rate, boot success, and sensor/system health during each deployment ring.
3. Stop the rollout automatically when monitoring detects abnormal failures.
4. Provide clear release notes and deployment controls so customers can select where and when the update is installed.

## 4. QA and QC comparison for this case

| Area | Example for the CrowdStrike case |
|---|---|
| QA - process-oriented prevention | Establish release governance, define validation criteria for configuration files, conduct independent review, and use staged deployment policies. |
| QC - product-oriented detection | Run automated validation, boundary tests, integration tests, crash monitoring, and canary deployment checks against the actual update. |

QA would reduce the chance that an unsafe update is approved for release. QC would identify the invalid update or its harmful behaviour before it reached the complete production fleet. Both are necessary.

## 5. Proposed improvements

1. Validate the number and structure of all configuration fields before publication.
2. Apply bounds checking in the content interpreter and handle invalid content safely.
3. Require automated regression tests for every template and configuration type.
4. Use progressive deployment rings: internal testing, canary customers, limited deployment, then broader rollout.
5. Monitor deployment telemetry and automatically pause a rollout when crash indicators exceed the threshold.
6. Test rollback procedures regularly and document the recovery runbook.

These proposals align with CrowdStrike's reported actions: stronger test procedures and validation, enhanced bounds checking, and deployment rings with acceptance checks.

## 6. GitHub and Git evidence to insert in the final report

Insert your own screenshots here when creating the PDF:

1. Screenshot of GitHub profile: `674295019-source`.
2. Screenshot of the private repository: `sqm_674295019`.
3. Screenshot of the terminal output from `git --version`.
4. Optional supporting screenshot: `git remote -v` showing that the local folder is linked to the repository.

## References

1. CrowdStrike. (2024, August 6). *Executive Summary: Root Cause Analysis - Channel File 291*. https://www.crowdstrike.com/wp-content/uploads/2024/08/Executive-Summary_Root-Cause-Analysis_Channel-File-291.pdf
2. CrowdStrike. (2024, July 24). *Preliminary Post Incident Review: Content Configuration Update Impacting the Falcon Sensor and the Windows Operating System (BSOD)*. https://www.crowdstrike.com/en-us/blog/falcon-content-update-preliminary-post-incident-report/

## AI-use record (complete only if required by the Lab 1 worksheet)

**Tool used:** OpenAI Codex  
**Prompt / task:** Requested help to locate reliable sources and organize an analysis of the CrowdStrike outage using software-quality concepts.  
**How the output was used:** Used only as an outline and drafting aid. Facts and references were checked against CrowdStrike's incident reports.  
**Student verification and changes:** [Describe what you verified, revised, or added yourself.]
