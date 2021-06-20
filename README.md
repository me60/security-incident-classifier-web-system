# security-incident-classifier-web-system

A front-end-only dynamic classification system with reusable, customisable logic elements designed for security incident classification systems. This system was created during my final year at the University of St. Andrews for my Senior Honours project, and acted as the subject of my dissertation. For a better understanding of the motivation of the project, how it is used, and examples of it in operation, look under the 'docs' directory for the full dissertation report, where that, plus a full documentation of the project, can be found.

This project was designed for use by IT Services at the University of St. Andrews. The aim of the system is to allow for the possibility to configure and mutate, easily, any bespoke security classification scheme with the ability to immediately deploy to a team of security analysts.

The system is designed in such a way that it can accommodate any combination of questions and answers, and how a specific answer chosen affects an end-severity for which there may be many. The motivation as well as increased reasoning of the following model can be found in the documentation. Questions have an off-screen mapping to Effects, which relate to final Severities displayed to the user at the bottom of the current form. Logic that governs the specific way in which Effects alter severities is controlled via how Effects are intermediately mapped to Severities, as such, they are referred to as Intermediates:

![](./src/assets/abstract_dynamic_severity_classification_system_diagram.PNG) 
