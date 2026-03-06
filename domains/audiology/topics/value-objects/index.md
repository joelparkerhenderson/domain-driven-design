# Value Objects in the Audiology Domain

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with identical attributes are considered equal and interchangeable. In the audiology domain, value objects capture the rich set of measurements, classifications, and clinical parameters that characterize hearing healthcare. Value objects are the primary mechanism for embedding domain knowledge and business rules into the model.

## Value Object Characteristics

Value objects in the audiology domain are immutable: once created, their attributes cannot change. If a different value is needed, a new value object is created. Value objects are self-validating: they enforce their own invariants at creation time, ensuring that invalid clinical values cannot exist in the domain model. Value objects are side-effect-free: operations on value objects produce new value objects without modifying existing ones.

## Hearing Assessment Value Objects

### HearingThreshold

The HearingThreshold value object represents a single measured threshold. It contains the frequency (in Hz), the intensity (in dB HL), the ear (left or right), the conduction type (air or bone), and the response indicator (response present or no response). The value object validates that frequency falls within the standard audiometric range (125 Hz to 16000 Hz), intensity falls within the audiometer's output limits, and the conduction type is valid for the specified frequency.

### Audiogram

The Audiogram value object represents the complete graphical record of hearing thresholds. It contains a collection of HearingThreshold value objects for both ears and both conduction types. The Audiogram validates that the threshold set is internally consistent and calculates derived values such as pure tone averages and hearing loss degree classifications.

### HearingLossClassification

The HearingLossClassification value object encapsulates the clinical classification of hearing loss. It contains the degree (normal, slight, mild, moderate, moderately severe, severe, profound), the type (conductive, sensorineural, mixed), and the configuration (flat, sloping, rising, notched, cookie-bite). Each component is validated against standard clinical definitions.

### MaskingParameters

The MaskingParameters value object captures the masking configuration used during audiometric testing. It contains the masking type (narrow band noise, speech noise), the masking level, and the target ear. The value object enforces masking rules based on interaural attenuation values and plateau verification requirements.

### PureToneAverage

The PureToneAverage value object represents the calculated average of hearing thresholds at 500, 1000, and 2000 Hz for a specific ear. It validates that all three contributing thresholds are present and calculates the arithmetic mean. This value object is used extensively for hearing loss degree classification and speech recognition testing.

## Device Management Value Objects

### FittingPrescription

The FittingPrescription value object represents the calculated amplification targets for a hearing device. It contains target gain values at each audiometric frequency, maximum power output limits, compression ratios, and the prescriptive formula used (NAL-NL2, DSL v5, or proprietary). The value object validates that targets are physically achievable and clinically appropriate.

### RealEarMeasurement

The RealEarMeasurement value object captures the probe microphone measurement of device output in the ear canal. It contains measured output values at each frequency, the measurement condition (REIG, REAR, RESR), and the deviation from prescriptive targets. The value object calculates match-to-target metrics.

### DeviceSpecification

The DeviceSpecification value object describes a hearing device's technical characteristics. It contains the manufacturer, model, style (BTE, RIC, ITE, CIC, IIC), technology level, number of channels, and wireless connectivity capabilities. The value object validates that the specified combination of attributes represents a real, available device configuration.

## Rehabilitation Value Objects

### RehabilitationGoal

The RehabilitationGoal value object defines a specific, measurable objective within a rehabilitation plan. It contains the goal description, the target metric, the baseline measurement, the target value, and the timeframe. The value object validates that goals are measurable and that target values represent clinically meaningful improvement.

### OutcomeMeasurement

The OutcomeMeasurement value object captures the result of a standardized outcome measure. It contains the instrument used (APHAB, HHIE, SSQ, IOI-HA), the scores for each subscale, the administration date, and the normative comparison. The value object validates that scores fall within the instrument's valid range and calculates composite scores according to the instrument's scoring algorithm.

## Vestibular Value Objects

### NystagmusRecording

The NystagmusRecording value object captures the parameters of recorded eye movements during vestibular testing. It contains the direction, velocity, frequency, and amplitude of nystagmus, along with the testing condition (spontaneous, positional, caloric). The value object validates physiologically plausible parameters.

### CaloricResponse

The CaloricResponse value object represents the vestibular response to caloric stimulation. It contains the slow phase velocity for each ear at each temperature (warm and cool), the calculated unilateral weakness percentage, and the directional preponderance. The value object enforces Jongkees formula calculations.

## Clinical Documentation Value Objects

### FrequencyRange

The FrequencyRange value object represents a range of audiometric test frequencies. It validates that the range contains standard audiometric frequencies and maintains proper ordering.

### DecibelLevel

The DecibelLevel value object represents a sound intensity measurement. It contains the numeric value and the reference scale (dB HL, dB SPL, dB SL). The value object validates that the value falls within physically meaningful limits for the specified scale.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 5 on value objects.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 6 on value objects.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 7 on value object patterns.
