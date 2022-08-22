# MoHSES Scenario File Format Specification

Scenario Files are XML formatted text documents that hold Scenario Metadata and Assigned Capabilities.
 This specification document illustrates how the XML is structured and standardized for MoHSES use.


## Scenario  ("Holds Metadata, Authors, and Capabilities.")

### Scenario properties

Section for the Scenario name and Authors of the scenario.

- Scenario Name

  - Name of the Scenario given by one of the document authors.

- Authors
  - List of authors that contributed to the Scenario File


## **Metadata** Definition and Structure

Property Types:
All properties are written as free-form text with certain validation rules for each.
For example: While "Name" is free-form text with no validation rules, "Age", "Height", and "Weight" must be written as whole number integers.


-
### Patient Properties:

- Name

  - Last, First

- Title or Rank

  - Type-in (candidate for including drop-down list)

- Age

  - Represented as a whole number in years.

- Gender

  - M or F

- Height [cm]

  - Represented as a whole number in centimeters.

- Weight [kg]

  - Represented as a whole number in kilograms.

-
### Environment Properties:

- Surrounding

  - String (up to 100 words)

- Altitude or Elevation

  - Whole number in meters

- Temperature [C]

  - Represented as a decimal number in Celsius. (can be negative)

- Pressure [mmHg]

  - Represented as a whole number plus one decimal point (xxx.x) in millimeters of mercury.

- Ambient CO2 [fraction]

  - Represented as a decimal, with up to 4 digits after decimal point (x.xxxx)

- Ambient Sounds

  - List of sounds as strings, with each sound description using one to three words

- Smells

  - List of smells as strings, with each smell description using one to three words

-
### Educational Encounter Properties

- Instructions
  - String, up to ~300 words
- Narrative
  - String, up to ~300 words
- Number of Learners
  - Integer between 1 and 9
- Roles
- Setup Checklist
- Equipment
- Supplies
- Estimated Duration
  - Integer number of minutes (0 to 99)
- Scenario Type
  - string
- Assessment Type
  - String
- Learner Group
  - String
- Learning Objectives
  - Ordered list of phrases (up to 10 phrases, up to 10 words/phrase)
- Task Descriptions
  - Ordered list of phrases (up to 20 phrases, up to 10 words/phrase)
- Performance Conditions
  - Ordered list of phrases (up to 10 phrases, up to 10 words/phrase)
- Exit Criteria
  - Ordered list of phrases (up to 10 phrases, up to 10 words/phrase)

**Capabilities **
List of Capabilities and their configurable data that can be assigned to this Scenario File.
 Each capability contains the following attributes:

- required

- _Whether or not this Capability is required by the scenario_

- name

- _The name of this Capability provided by the Operational Description_

- Configuration Data

- _Container for Capability Data._

- Data

- _A Capability property that is configurable._
- name

- _The name of this property._

- type

- _The data type this property is supposed to be treated as. Could be "string", "number", "boolean", or "option."_

- "_string": Standard UTF8 free-form text without a char limit._
- "_number": Standard 32-bit floating-point number._
- "_boolean": Contains the explicit text "true" or "false" capitalization ignored._
- "_option": A pre-defined text value selectable from a pre-defined group._

- value

- _The set value of the property respecting the defined data type._

- Option

- _List element that contains all the pre-defined options if this type is "option"._
- _A pre-defined text value that can be selected to be the value of this property._
