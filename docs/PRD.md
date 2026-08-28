# Product Requirements

## Problem Definition

The City of Winnipeg needs a reliable way to combine historical energy utility consumption, cost and weather data. The platform will help data professionals and decision makers understand historical and current energy usage patterns, identify major cost and consumption drivers, analyze changes over time, forecast future consumption and cost to support budgeting and future energy planning.

## Consumers and Their Needs

**Data Analysts:** Reliable data to map out trends, KPIs, compare and contrast what has been and what currently is. Analysts can investigate patterns and provide explanations that other stakeholders can act on.

**Data Scientists / ML Engineers:** Feature-ready historical data to build predictive products that improve decision making, including demand and cost forecasting and anomaly detection.

**Finance Team:** Uses the platform to understand consumption, cost and forecast to plan and allocate resources.

**Energy and Operations Team:** Assess usage patterns, high-consumption areas, anomalies and where opportunities for improved efficiency may exist in the system.

**Policy / Strategy Team:** Platform provides patterns and evidence that helps them evaluate energy initiatives and policies.

**Urban Planning Team:** Where supported by location and building data, it can provide geographic and building- level insights to support energy-efficient urban planning.

## Business Questions

1. **Is something happening that shouldn't be?** Are there unusual changes or patterns in consumption or cost that warrant investigation?

2. **Why are we spending this much?** What are the major drivers of energy cost?

3. **How has our energy use changed over time?**

4. **Where is the energy going?** Which accounts/service locations/service types consume the most energy?

5. **Considering Winnipeg's extreme climate, how much does our weather affect energy consumption and cost?**

6. **How much energy are we likely to use in the future and how much might it cost us?**

7. **Where should we do better?** Which consumption and cost patterns show areas that may warrant investigation for improved efficiency?

## Non-Functional Requirements

This section describes how the platform should behave while also taking note of the business capabilities it provides. Essentially, explaining not just what the platform should do but how well and under what constraints it should be done. 

This is provisional and would be refined as the project is better understood.

- **Reliability and correctness:** Essentially asks "Can I trust what the system produced?" The platform should process data consistently and prevent failures or unexpected source changes from silently producing incorrect downstream data.

- **Maintainability and evolvability:** Esssentially asks "Can the system be changed without issues?" Components, transformations, and data models should be understandable and modifiable as requirements and source schemas change.

- **Recoverability:** Essentially asks "If something breaks, can I recover?" Derived datasets should be reproducible from preserved data where practical, and failed processing should be capable of being safely retried.

- **Security:** Essentially says "Only the right people/services should be able to access the right things." Access to data, infrastructure, credentials, and services should follow least-privilege principles. Secrets should not be stored directly in source code.

- **Cost efficiency:** Essentially asking "is the complexity/resource usage justified?" Architecture and cloud services should correspond to actual workload. Resources or infrastructure that do not add value should not be introduced "just because".

## Platform Outputs

### Refined Data Layer

#### Purpose

Provides clean, standardized, integrated and analysis-ready data for downstream use. The platform preserves its source data in its raw form for traceability and reproducibility.

#### Grain

Based on the first review of data, one row represents one utility service record for a specific account/meter, service type, service location and billing period. This is provisional.

#### Dimensions

**When?**
- Time
    - billing period
    - month
    - quarter
    - year
    - season

**Where?**
- Location
    - service address
    - geographical area, if a reliable mapping can be established.

**What?**
- Service
    - utility/service type
    - account/meter

**Who?**
- Organization
    - department, if a reliable mapping can be established.

**Context?**
- Weather
    - Temperature
    - Precipitation
    - Other weather-derived variables where useful

#### Measures


**How much energy?**
- Energy Consumption
    - Billing units

- Service Period
    - Days of service

**How much money?**
- Cost
    - Amount due

    **Why that cost?**
    - Basic charges
    - Distribution charges
    - Transportation charges
    - Demand charges
    - Carbon charges
    - Taxes
    - Adjustments

#### Derived Metrics
This would include but not limited to:

- Cost per billing unit
- Consumption per service day
- Cost per service day
- Monthly and annual consumption
- Monthly and annual expenditure
- Period-over-period consumption change
- Period-over-period cost change
- Cost-component share of total expenditure

#### Freshness

The platform should check for new utility and weather data daily because the platform doesn't control when new records are published. 

This is to pick up fresh data without assuming a specific schedule. Downstream datasets would be updated accordingly and maintain idempotency. 

This refresh strategy may be revised later after adequate profiling of publication frequency is done.

#### Quality Assurance

- **Uniqueness:** Duplicate utility service records should be identified and prevented.

- **Schema validation:** Incoming fields should conform to their expected data types and formats. Unexpected schema changes should be detected and prevented from silently propagating downstream. Where safe, known changes may be handled automatically; otherwise the pipeline should fail clearly and require review.

- **Completeness:** Unexpected missingness should be detected and reported. Records should be handled according to documented rules rather than silently discarded or imputed.

- **Validity:** Values should conform to documented business and structural rules, such as expected ranges, formats, allowable values, and logical relationships.

- **Freshness:** The platform should track source updates and identify unexpected delays or failures.

- **Consistency:** Relationships between records and downstream dimensions should remain valid as the data moves through the platform.

- **Data Dictionary:** The platform is accompanied by a document that details important fields, units, codes, transformations, known values, and relevant assumptions. Unknown ones should be investigated and verified before documentation.

## Downstream Products

This data will support downstream products that are designed around stakeholder needs.

### Product 1: Energy Intelligence & Reporting

Supports historical analysis, KPI reporting, cost analysis, consumption
trends, weather relationships, and investigation by analysts, finance,
energy/operations, and other decision-makers.

### Product 2: Energy Forecasting

Provides forecasts of future energy consumption and cost to support
budgeting, resource planning, and energy planning.

### Product 3: Energy Monitoring & Investigation

Identifies unusual consumption or cost patterns and provides the context
needed for analysts and operations teams to investigate them.
















