# Units of measure (UCUM)

## What is UCUM and why?

There are multiple ways to specify a unit of measure in a software-interpretable message, which can cause incompatibility. It is not always clear what a particular symbol means. For example, "C" could refer to "Celsius" or "Coulomb", and a human being could come up multiple ways to express Celsius values, such as: "C", "deg_C", "DegC", "Cel" or "Celsius". Even if a human reader understood the meaning, this often does not apply to software. In principle, this challenge applies to any unit of measure.

This is what The Unified Code for Units of Measure (UCUM) is for. UCUM is introduced as follows (from [https://ucum.org/trac](https://ucum.org/trac)):

> The Unified Code for Units of Measure (UCUM) is a code system intended to include all units of measures being contemporarily used in international science, engineering, and business. The purpose is to facilitate unambiguous electronic communication of quantities together with their units. The focus is on electronic communication, as opposed to communication between humans.

> The Unified Code for Units of Measures -- has already been adopted by some standard organizations such as DICOM, HL7 and has been referenced as best practice by the Open Geospatial Consortium in their Web Map Service (WMS) and Geography Markup Language (GML) implementation specifications.

UCUM is not the first specification to encode units of measure. However, UCUM is the best solution known to exist. The advantages include:

- openly available at no cost
- coverage (in principle, you can express any unit of measure in the world)
- human-readability and intuitiveness (in contrast to some specifications that use numeric identifiers, such as UNECE: http://tfig.unece.org/contents/recommendation-20.htm)


## UCUM in SimCES

All fields that express a unit of measure SHOULD comply with the case-sensitive UCUM format (see [https://ucum.org/ucum.html](https://ucum.org/ucum.html)).

In the tables of UCUM specification, these are defined in the column "c/s".


## Units possibly beneficial in simulation

UCUM is not limited to these units, but these can be beneficial.

| Category | Name | UCUM presentation | Comment |
|-|-|-|-|
| General | per unit | {pu} | The unit is effectively "1", which is the default unit. However, curly braces enclose an annotation that elaborates what the value means. "EUR" is a currency code standardized in ISO 4217. |
| Current | ampere | A | |
| Energy | megawatt hour | MW.h | |
| Percentage | Percentage | % | |
| Power (apparent) | kilovolt ampere | kV.A | |
| Power (reactive) | kilovolt ampere (reactive) | kV.A{r} | |
| Power (real) | megawatt | MW | |
| Price | per megawatt hour | {EUR}/(MW.h) | Currencies are not supported in UCUM. Therefore, "EUR" is merely an annotation here (i.e., in curly brackets) and means "1". |
| Temperature | celsius | Cel | |
| Voltage | kilovolt | kV | |


## Unit rules

### Basic

UCUM writes many of the typical units as we would think. E.g.:, meter: "m", second: "s", Joule: "J".

However, some unit can cause ambiquity and therefore have another definition. E.g.:

Celsius: "Cel", pH: "[pH]".


### Prefixes

Prefixes are supported. E.g.: tera: "T", mega: "M", kilo: "k".

More prefixes: see §27 "prefixes" at [https://ucum.org/ucum.html#para-27](https://ucum.org/ucum.html#para-27).


### Square brackets

At [https://ucum.org/ucum.html#para-14](https://ucum.org/ucum.html#para-14):

> §14 square brackets. Square brackets enclose suffixes of unit symbols that change the meaning of a unit stem.

For example:

| UCUM c/s presentation | Meaning |
|-|-|
| mm[Hg] | millimeter Mercury column |
|[ppm]| Parts per million |


### Curly braces

At [https://ucum.org/ucum.html#para-12](https://ucum.org/ucum.html#para-12):

> §12 curly braces. Curly braces may be used to enclose annotations that are often written in place of units or behind units but that do not have a proper meaning of a unit and do not change the meaning of a unit.

> For example one can write “%{vol}”, “kg{total}”, or “{RBC}” (for “red blood cells”) as pseudo-units. However, these annotations do not have any effect on the semantics, which is why these example expressions are equivalent to “%”, “kg”, and “1” respectively.
