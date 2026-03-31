# Dataiku v12/v13 to v14 Upgrade planning toolkit

Preparing for a major version upgrade requires a detailed inventory of your current instance, and that can be difficult to do manually so I wrote a plugin and built a project to assist.

The Upgrade Planning Toolkit assists DSS Administrators by automating the discovery of objects that may require attention during an upgrade. It utilizes the [Datasets about Dataiku Instance plugin](https://github.com/THE-MOLECULAR-MAN/dss-plugin-xzibit) to surface deep metadata about your instance, helping you:
* **Prioritize Migration**: Identify which projects are most active and should be moved first.
* **Identify Deprecations**: Locate specific objects (Recipes, Plugins, Code Environments) that may break due to version changes.

[This video](https://www.youtube.com/watch?v=t2wDjbikAgE) gives an overview. 

This plugin is not officially supported by Dataiku. It should only be used on Design nodes, not automation or API nodes, and should not be used in any mission critical environments.


# Upgrade Considerations
Review the major changes between your current version and the target version. Dataiku acts as an orchestration layer; consequently, when underlying technologies end support, DSS must follow suit.

**Key areas to review:**

* **Release Notes:** Review [Dataiku's Release Notes](https://doc.dataiku.com/dss/latest/release_notes/) specifically for **Migration Paths** and **Limitations**.
 -  [Changes in DSS v13](https://doc.dataiku.com/dss/latest/release_notes/13.html#limitations-and-warnings)
 - [Changes in DSS v14](https://doc.dataiku.com/dss/latest/release_notes/14.html#limitations-and-warnings)
* **Python/R Deprecation:** E.g., The Python Foundation ended support for Python 3.6 in 2021.
* **OS Compatibility:** Ensure your Linux VM's OS version is supported by the new DSS version. This is handled by Dataiku Fleet Manager. If some of your custom code environments required additional OS packages, be aware that you may need to update os package names in your Ansible template in Fleet Manager.
* **Plugin Compatibility:** Some older plugins may be deprecated or require updates. Often times these plugins have been replaced by superior in-product features.
* **Container Images:** Base images for Docker/Kubernetes may need updating.


## Analysis Dashboard
Once the build is complete, navigate to the **Dashboards** tab in this project.

The dashboard provides visual breakdowns of your instance, allowing you to answer questions such as:
* *Which projects have been modified in the last 6 months?*
* *Which code environments are using deprecated Python versions?*
* *Which plugins need to be updated?*


## Out of scope
This project does not assist with the following:
- gathering information/configuration about the underlying operating system's packages - for example: extra packages installed in Alma Linux to support custom plugins
- listing Docker images used
- providing a list of deprecated features - see [DSS Release Notes and their deprecation notices]
- searching custom code (Python, R, SQL, etc) for any potential upgrade/migration issues


# Installation instructions

[This video](https://www.youtube.com/watch?v=t2wDjbikAgE) walks through the install process step-by-step.

**The latest version does not require a PostgreSQL deployment anymore, so you can ignore mentions of PostgreSQL.**

## Step 1: Importing the plugin
1) [Visit the Datasets about Dataiku Plugin release page](https://github.com/THE-MOLECULAR-MAN/dss-plugin-xzibit/releases)
2) Find the latest release for DSS version 12 (not version 14). Click on Source code (zip) to download the file.
3) It should download a file named something similar to dss-plugin-xzibit-DSSv12_plugin_v1.0.0.zip
4) In DSS v12, select the Administration menu, then Plugins
5) Add Plugin > Upload
6) Attach the ZIP file you downloaded in step 3.
7) On the next page, click Build New Environment.

## Step 2: Importing the project
1) Download the DSS Project sent from your Sales Engineer. For example: DSS_v12_Upgrade_Planning_Toolkit-v1.0.0.zip . Note that this is a different file from the plugin.
2) On the homepage of DSS v12, click the blue New Project button, then Import Project.
3) Click Choose File and select the project zip file from step one.
4) Click the blue Import button
5) You may see a warning about other plugins that were installed - you can ignore this.

## Step 3: Using the project
Open the project and visit its Wiki page for instructions on how to build the dashboards.

---

# Limitations, Permissions, and Access
> **⚠️ Important Disclaimers**
> * **Non-Production Use Only:** This project and the associated plugin are intended to be run on **Development/Sandbox** systems. It has not been QA'd for Production environments.
> * **Best-Effort Support:** This project is a custom asset and is **not** officially supported by Dataiku Product Support.
> * **Target Audience:** This tool is designed for **DSS Administrators** planning a migration (e.g., v12 -> v14+). It assumes mild-to-moderate DSS experience but does not require coding skills.
> * **RBAC Considerations:** This toolkit produces datasets that describe admin level metadata about a Dataiku DSS installation. If this plugin's datasets are pushed into other datasets, then metadata about those other resources (datasets, recipes, models, etc) may be visible by users who would not normally be able to see those resources.
