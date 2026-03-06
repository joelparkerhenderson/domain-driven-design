# Device Integration Context

## Overview

The Device Integration Context handles data ingestion, normalization, and translation from external sleep-related devices into domain-standard representations. Sleep quality management increasingly relies on data from consumer wearables, smart mattresses, CPAP therapy machines, and ambient environment sensors. Each device manufacturer uses proprietary data models, units, terminology, and quality characteristics. This context provides the anti-corruption layers necessary to protect downstream bounded contexts from device-specific complexity.

## Core Responsibilities

This context owns the domain logic for external device relationship management and data translation. It maintains device registrations and connection configurations. It ingests raw data from device APIs and manufacturer data platforms. It applies device-specific translation rules to normalize data into canonical domain formats. It assesses data quality and annotates normalized data with confidence metadata. It publishes normalized data events for consumption by downstream contexts.

## Supported Device Categories

### Consumer Wearables

Consumer wearables include fitness trackers and smartwatches from manufacturers such as Fitbit, Apple Watch, Garmin, Samsung Galaxy Watch, and Oura Ring. These devices estimate sleep-wake patterns through accelerometry and heart rate monitoring. They typically provide estimates of total sleep time, sleep stages (light, deep, REM), sleep onset time, wake time, and nighttime movement. The context normalizes these manufacturer-specific stage labels to domain-standard terminology (N1/N2 for light sleep, N3 for deep sleep, REM for REM sleep) and annotates all estimates with appropriate confidence levels, as consumer wearables have lower accuracy than clinical polysomnography.

### Smart Mattresses and Sleep Trackers

Smart mattresses and under-mattress sleep trackers (such as Withings Sleep Analyzer, Eight Sleep, and SleepScore Max) detect sleep through ballistocardiography, pressure sensing, and other non-contact technologies. They provide data on sleep-wake patterns, heart rate, respiratory rate, and bed occupancy. The context normalizes these data types and handles the differences in measurement methodology compared to wearable devices.

### CPAP and BiPAP Machines

CPAP and BiPAP therapy machines from manufacturers such as ResMed, Philips Respironics, and Fisher & Paykel generate therapy compliance and efficacy data. Key data elements include nightly usage hours, AHI during therapy, mask leak rates, pressure settings and adjustments, and therapy events (apneas, hypopneas, flow limitations). The context translates device-specific event classifications and metrics into domain-standard representations aligned with the Sleep Disorders Context model.

### Ambient Environment Sensors

Smart home sensors and dedicated sleep environment monitors capture light levels, noise levels, temperature, humidity, and air quality in the sleeping environment. Sources include Philips Hue, Nest, Awair, and dedicated sleep environment monitors. The context normalizes sensor readings into standardized units (lux, decibels, degrees Celsius/Fahrenheit, percent relative humidity) and temporal granularity for consumption by the Sleep Environment Context.

## Key Aggregates

### Device Feed

The primary aggregate manages the relationship between an external device and the domain. The root entity tracks the device type, manufacturer, model, serial number or identifier, API connection configuration, data format version, translation rule set, connection status (active, paused, disconnected, error), and last successful ingestion timestamp. The aggregate contains DataIngestion entities representing individual data retrieval sessions, with their status, data volume, quality metrics, and any errors encountered.

## Anti-Corruption Layer Design

Each supported device type has a dedicated anti-corruption layer (ACL) translator. The ACL handles field mapping (mapping device-specific field names to domain concepts), unit conversion (converting device-specific units to domain-standard units), temporal alignment (normalizing timestamps to UTC and aligning temporal granularity), quality annotation (assessing and annotating data reliability based on device capabilities), gap handling (detecting and flagging missing data periods), and error translation (converting device-specific error codes to domain-meaningful error descriptions).

## Data Quality Model

Not all device data is equally reliable. Consumer wearables estimate sleep stages with lower accuracy than polysomnography. CPAP machines provide highly accurate therapy usage data but may have less reliable automatic event scoring for complex respiratory events. The context models data quality along several dimensions: measurement accuracy (how close the device measurement is to a gold-standard reference), temporal resolution (how fine-grained the data points are), completeness (whether data covers the full sleep period), and consistency (whether the device produces stable results under stable conditions).

## Domain Events Published

The context publishes DeviceDataReceived when normalized data is available, carrying the device type, data type, patient identifier, timestamps, and the normalized data payload. It publishes DeviceConnectionStatusChanged when a device feed's connection status transitions, enabling downstream contexts to account for data availability changes.

## Relationships with Other Contexts

The Device Integration Context acts as an upstream supplier to multiple downstream contexts. The Sleep Assessment Context receives actigraphy and wearable-derived sleep metrics in a conformist relationship. The Sleep Environment Context receives normalized sensor data. The Outcomes Tracking Context receives device-derived metrics for longitudinal analysis. The Sleep Disorders Context may receive CPAP therapy data relevant to OSA management.

## Vendor Isolation

A critical design principle is that no device vendor concepts, terminology, or data structures escape the Device Integration Context boundary. Downstream contexts never work with Fitbit sleep stages, ResMed event types, or Withings sleep scores. They work exclusively with domain-standard value objects produced by the anti-corruption layers. This isolation ensures that adding a new device manufacturer or updating an existing API affects only this context.

## Data Ingestion Patterns

The context supports multiple data ingestion patterns: pull-based polling of device APIs on configurable schedules, push-based webhook reception for devices that support real-time notification, bulk import for historical data migration, and manual upload for devices without API connectivity. The ingestion pattern is configured per device feed and is transparent to downstream consumers.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on anti-corruption layers.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 13 on integration.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 9 on integration patterns.
- Depner, C.M. et al. (2020). Wearable Technologies for Developing Sleep and Circadian Biomarkers. Sleep, 43(1), zsz197.
