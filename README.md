# Vigil — Detecting Suspicious Configuration Changes

**Vega Systems Internship · Python · Synthetic Data Generation · Anomaly Detection · PyOD · C#/.NET · Milestone XProtect SDK**

During my Software Engineering Internship at **Vega Systems Inc.**, I contributed to work on **Vigil**, Vega's product for detecting suspicious configuration changes in video management systems (VMS). A VMS manages security cameras and the settings that control how a deployment operates, including camera configurations, recording, user permissions, alarms, and video analytics.

The challenge behind Vigil is that configuration changes happen routinely. Most are legitimate administrative activity, but some may be unexpected or suspicious. The goal is to help identify changes that stand out from normal activity and may warrant investigation.

My work contributed to this effort in two connected areas: generating synthetic examples of normal configuration activity so anomaly-detection methods could be explored, and using the Milestone XProtect SDK with C#/.NET to retrieve configuration logs. The broader objective was to bring log retrieval and anomaly detection together so that retrieved changes could be checked for suspicious behavior.

- **Role:** Software Engineering Intern
- **Organization:** Vega Systems Inc.
- **Location:** San Jose, California
- **Engineering focus:** Synthetic data generation, anomaly detection, and C#/.NET SDK development

This repository is a technical overview of my contributions to a larger product effort. It is not a description of Vigil's complete implementation and does not reproduce company-owned source code, internal documentation, or customer data.

---

## The problem

Configuration changes are a normal part of operating a VMS. Administrators may install or decommission cameras, tune camera settings, change recording behavior, update user roles, or modify alarm and analytics configurations. These changes can affect what a system records, who can access it, and how it responds to events.

The difficult part is distinguishing expected maintenance from changes that are unusual enough to deserve a closer look. Treating every change as suspicious would generate too many alerts. An anomaly-detection approach instead looks for activity that differs from patterns of normal system operation.

The project therefore needed two things: representative examples of normal configuration activity to develop and explore detection methods, and a way to retrieve configuration logs from an actual XProtect environment for analysis.

## 1. Generating synthetic data for normal configuration activity

A key challenge was the **lack of publicly available data describing normal VMS configuration activity**. Without representative examples of routine changes, it was difficult to establish what normal behavior looked like or explore anomaly-detection methods against it.

I contributed a **Python synthetic-data generation pipeline** that creates configuration-change records representing normal activity in a Milestone XProtect-style environment.

The generator models several types of routine administrative activity:

- Camera installation, decommissioning, and configuration tuning
- Recording configuration changes
- User-role and permission changes
- Alarm configuration changes
- Video analytics configuration changes

The pipeline generates these activities over a simulated eight-week period. By representing different categories of routine configuration changes, it provides a controlled dataset that can be used to examine patterns of normal system activity.

## 2. Exploring anomaly detection

With representative normal-activity data available, I worked with two anomaly-detection approaches to explore how unusual configuration changes could be identified.

### Isolation Forest (PyOD)

I used PyOD's **Isolation Forest** implementation to identify observations that differ from the broader dataset.

Isolation Forest works by repeatedly partitioning data and measuring how easily individual observations can be isolated. Observations that are isolated more easily than typical examples receive higher anomaly scores.

Applied to configuration-change data, this provides a way to identify records with unusual patterns that may warrant further investigation.

### Autoencoder

I also explored an **autoencoder**, a neural network that learns to reconstruct patterns in its input data.

An autoencoder learns representations of the data and attempts to reconstruct its inputs. The reconstruction error provides another signal for identifying unusual observations. Changes that do not fit the learned patterns as well can produce larger reconstruction errors and be flagged for review.

### Comparing the approaches

The two methods provide different ways to identify activity that differs from the generated normal-configuration data. Isolation Forest evaluates how readily an observation can be separated from others, while an autoencoder uses reconstruction error as an indicator of unusual patterns.

In my experiments, the **autoencoder was substantially more effective than Isolation Forest** at identifying unusual configuration changes in the generated data. Based on these results, the autoencoder was the more promising approach for this use case. 

## 3. Retrieving configuration logs with the Milestone XProtect SDK

Synthetic data provided examples of normal configuration activity, but the broader objective also involved retrieving configuration changes from an actual VMS environment.

I used the **Milestone XProtect SDK** with **C#/.NET** to retrieve configuration logs through its log-reading functionality. This work involved a C#/.NET console application using the SDK.

The development environment included:

- C#
- .NET Framework 4.7.2
- Milestone XProtect SDK 26.1.2

This provided a way to access configuration-log information from XProtect rather than relying exclusively on generated records. It addressed a separate but important part of the overall problem: obtaining the configuration activity that the detection workflow was intended to analyze.

## 4. Connecting log retrieval with anomaly detection

The broader objective was to create a workflow in which configuration logs could be retrieved from Milestone XProtect and checked for suspicious activity.

The intended process was:

1. **Retrieve configuration logs:** Use the Milestone XProtect SDK to access configuration-change activity.
2. **Analyze configuration changes:** Use anomaly-detection methods to evaluate how the retrieved activity compares with patterns of normal configuration behavior.
3. **Identify unusual activity:** Surface changes that appear unusual for further investigation.

The synthetic-data pipeline provided representative examples of normal configuration activity for anomaly-detection development, while the C#/.NET application retrieved configuration logs from XProtect to support the goal of evaluating real system changes for suspicious behavior.

## Tools and technologies

| Area                      | Tools and technologies              |
| ------------------------- | ----------------------------------- |
| Programming               | Python, C#                          |
| Synthetic data generation | Python data-generation pipeline     |
| Anomaly detection         | PyOD, Isolation Forest, Autoencoder |
| VMS platform              | Milestone XProtect                  |
| Log retrieval             | Milestone XProtect SDK 26.1.2       |
| .NET development          | .NET Framework 4.7.2                |

## Role and contribution

As a Software Engineering Intern, I contributed to the broader Vigil effort by developing a Python pipeline to generate synthetic examples of normal configuration activity, exploring Isolation Forest and autoencoder approaches for identifying unusual changes, and using the Milestone XProtect SDK with C#/.NET to retrieve configuration logs.

The assignment gave me experience working across data generation, anomaly detection, and SDK-based log retrieval while applying those techniques to a practical security-software problem.

## Scope and disclosure

This repository is a portfolio overview of my internship contributions. It does not include company-owned source code, private repository contents, customer data, production logs, real customer configurations, credentials, or internal documentation.

---

*Vega Systems · Vigil · Software Engineering Internship · Suspicious Configuration-Change Detection*
