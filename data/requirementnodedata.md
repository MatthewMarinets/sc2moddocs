# Requirement Node Data
Defines requirement nodes, which are individual nodes in an expression used as part of a requirement.

<details>
<summary>Table of Contents</summary>

- [Requirement Node Data](#requirement-node-data)
  - [Common fields](#common-fields)
    - [Flags](#flags)
    - [Tooltip](#tooltip)
  - [Enums](#enums)
    - [State](#state)
  - [Types](#types)
    - [CRequirementAllowAbil](#crequirementallowabil)
    - [CRequirementAllowBehavior](#crequirementallowbehavior)
    - [CRequirementAllowUnit](#crequirementallowunit)
    - [CRequirementAllowUpgrade](#crequirementallowupgrade)
    - [CRequirementCountAbil](#crequirementcountabil)
    - [CRequirementCountBehavior](#crequirementcountbehavior)
    - [CRequirementCountUnit](#crequirementcountunit)
    - [CRequirementCountUpgrade](#crequirementcountupgrade)
    - [CRequirementNot](#crequirementnot)
    - [CRequirementOdd](#crequirementodd)
    - [CRequirementAnd](#crequirementand)
    - [CRequirementOr](#crequirementor)
    - [CRequirementXor](#crequirementxor)
    - [CRequirementEq](#crequirementeq)
    - [CRequirementNE](#crequirementne)
    - [CRequirementLT](#crequirementlt)
    - [CRequirementGT](#crequirementgt)
    - [CRequirementLTE](#crequirementlte)
    - [CRequirementGTE](#crequirementgte)
    - [CRequirementDiv](#crequirementdiv)
    - [CRequirementMod](#crequirementmod)
    - [CRequirementMul](#crequirementmul)
    - [CRequirementConst](#crequirementconst)
    - [CRequirementSum](#crequirementsum)

</details>

## Common fields
All requirement nodes have the following fields:

### Flags
* attribute `index`: "TechTreeCheat"
* attribute `value`: 1

### Tooltip
* attribute `value`: gamestrings.txt key
* Controls the tooltip used if a requirement is displayed in a tooltip

## Enums
### State
This enum is used to filter the state of count-type requirement nodes. Values are:
* CompleteOnly
* CompleteOnlyAtUnit
* InProgressOnly
* InProgressOnlyAtUnit
* InProgressOrBetter
* InProgressOrBetterAtUnit
* Killed
* Kills
* Peak
* (Queued (default behaviour))
* QueuedOnlyAtUnit
* QueuedOrBetter
* QueuedOrBetterAtUnit
* QueuedOrBetterOrRevivable
* Revivable
* Total

## Types
### CRequirementAllowAbil
* field `Link`:
  * attribute `value`: `CAbil` id -- the ability to check
* field `Index`:
  * attribute `value`: int -- the ability index
* Returns 1 if the ability is allowed / unlocked by triggers

### CRequirementAllowBehavior
* field `Link`:
  * attribute `value`: `CBehavior` id -- the behavior to check
* Returns 1 if the behavior is allowed / unlocked by triggers

### CRequirementAllowUnit
* field `Link`:
  * attribute `value`: `CUnit` id -- the unit to check
* Returns 1 if the unit is allowed / unlocked by triggers

### CRequirementAllowUpgrade
* field `Link`:
  * attribute `value`: `CUpgrade` id -- the upgrade to check
* Returns 1 if the upgrade is researchable / unlocked by triggers

### CRequirementCountAbil
* field `Count`:
  * attribute `Link`: `CBehavior` id
  * attribute `State`: enum -- [State](#state)
  * attribute `Unlock`: string (optional)
* Returns the current level of an ability
* 0-indexed, so the first level counts as 0

### CRequirementCountBehavior
* field `Count`:
  * attribute `Link`: `CBehavior` id
  * attribute `State`: enum -- [State](#state)
  * attribute `Unlock`: string (optional)
* Returns the number of stacks of a behaviour specified by `Count.Link`, and matching `State`
* Unsure what `Unlock` does

Example:
```xml
<CRequirementCountBehavior id="AP_CountBehaviorTyrannozorMergeSupplyCompleteOnlyAtUnit">
    <Count Link="AP_DisableTyrannozorMorphlingMergeSupply" State="CompleteOnlyAtUnit"/>
</CRequirementCountBehavior>
```

### CRequirementCountUnit
* field `Count`:
  * attribute `Link`: `CUnit` id
  * attribute `State`: enum -- [State](#state)
  * attribute `Unlock`: string (optional)
* Counts the number a unit type owned by a player

### CRequirementCountUpgrade
* field `Count`:
  * attribute `Link`: `CUpgrade` id
  * attribute `State`: enum -- [State](#state)
  * attribute `Unlock`: string (optional)
* Counts the level of an upgrade researched by a player

### CRequirementNot
* field `OperandArray`:
  * attribute `index`: 1
  * attribute `value`: `CRequirementNode` id
* Returns 1 if the requirement specified by `OperandArray[0].value` returns 0; else returns 0

Example:
```xml
<CRequirementNot id="AP_NotCountUpgradeSupplyDepotDropCompleteOnly">
    <OperandArray index="0" value="AP_CountUpgradeSupplyDepotDropCompleteOnly"/>
</CRequirementNot>
```

### CRequirementOdd
* field `OperandArray`
  * attribute `index`: 1
  * attribute `value`: `CRequirementNode` id
* Returns 1 if the requirement specified by `OperandArray[0].value` returns an odd number; else returns 0

### CRequirementAnd
* field `OperandArray`:
  * attribute `value`: `CRequirementNode` id
  * Array; can occur multiple times
* Returns 1 if all of the requirements specified by `OperandArray[*].value` return non-zero numbers; else returns 0

### CRequirementOr
* field `OperandArray`:
  * attribute `value`: `CRequirementNode` id
  * Array; can occur multiple times
* Returns 1 if any of the requirements specified by `OperandArray[*].value` return non-zero numbers; else returns 0

### CRequirementXor
* field `OperandArray`:
  * attribute `index`: 0 | 1
  * attribute `value`: `CRequirementNode` id
  * Flag type; can occur once for each index
* Returns the logical xor of `OperandArray[0].value` and `OperandArray[1].value`

### CRequirementEq
* field `OperandArray`:
  * attribute `index`: 0 | 1
  * attribute `value`: `CRequirementNode` id
  * Flag type; can occur once for each index
* Returns 1 if `OperandArray[0].value` and `OperandArray[1].value` are equal; else returns 0

Example:
```xml
<CRequirementEq id="AP_EqCountUpgradeProtossAirArmorsLevel1CompleteOnly1">
    <OperandArray index="0" value="AP_CountUpgradeProtossAirArmorsLevel1CompleteOnly"/>
    <OperandArray index="1" value="1"/>
</CRequirementEq>
```

### CRequirementNE
* field `OperandArray`:
  * attribute `index`: 0 | 1
  * attribute `value`: `CRequirementNode` id
  * Flag type; can occur once for each index
* Returns 1 if `OperandArray[0].value` and `OperandArray[1].value` are not equal; else returns 0

### CRequirementLT
* field `OperandArray`:
  * attribute `index`: 0 | 1
  * attribute `value`: `CRequirementNode` id
  * Flag type; can occur once for each index
* Returns 1 if `OperandArray[0].value` < `OperandArray[1].value`; else returns 0

### CRequirementGT
* field `OperandArray`:
  * attribute `index`: 0 | 1
  * attribute `value`: `CRequirementNode` id
  * Flag type; can occur once for each index
* Returns 1 if `OperandArray[0].value` > `OperandArray[1].value`; else returns 0

### CRequirementLTE
* field `OperandArray`:
  * attribute `index`: 0 | 1
  * attribute `value`: `CRequirementNode` id
  * Flag type; can occur once for each index
* Returns 1 if `OperandArray[0].value` <= `OperandArray[1].value`; else returns 0

### CRequirementGTE
* field `OperandArray`:
  * attribute `index`: 0 | 1
  * attribute `value`: `CRequirementNode` id
  * Flag type; can occur once for each index
* Returns 1 if `OperandArray[0].value` >= `OperandArray[1].value`; else returns 0

### CRequirementDiv
* field `OperandArray`:
  * attribute `index`: 0 | 1
  * attribute `value`: `CRequirementNode` id
  * Flag type; can occur once for each index
* Returns the quotient `OperandArray[0].value` / `OperandArray[1].value`

### CRequirementMod
* field `OperandArray`:
  * attribute `index`: 0 | 1
  * attribute `value`: `CRequirementNode` id
  * Flag type; can occur once for each index
* Returns the modulo `OperandArray[0].value` % `OperandArray[1].value`

### CRequirementMul
* field `OperandArray`:
  * attribute `value`: `CRequirementNode` id
  * Array; can occur multiple times
* Returns the product of all the evaluations of requirements in `OperandArray[*].value`

### CRequirementConst
* field `Value`:
  * attribute `value`: integer
* Defines a constant for use with comparisons / math operators

Example:
```xml
<CRequirementConst id="1">
    <Value value="1"/>
</CRequirementConst>
```

### CRequirementSum
* field `OperandArray`:
  * attribute `value`: `CRequirementNode` id
  * Array; can occur multiple times
* Returns the sum of all the evaluations of requirements in `OperandArray[*].value`

Example:
```xml
<CRequirementSum id="SumUnitQueen3QueuedOrBetterUnitQueen3BurrowedQueuedOrBetterUnitQueen2QueuedOrBetterUnitQueen2BurrowedQueuedOrBetterUnitQueenQueuedOrBetterUnitQueenBurrowedQueuedOrBetterUnitQueenCocoonQueuedOrBetter">
    <OperandArray value="UnitQueen3QueuedOrBetter"/>
    <OperandArray value="UnitQueen3BurrowedQueuedOrBetter"/>
    <OperandArray value="UnitQueen2QueuedOrBetter"/>
    <OperandArray value="UnitQueen2BurrowedQueuedOrBetter"/>
    <OperandArray value="UnitQueenQueuedOrBetter"/>
    <OperandArray value="UnitQueenBurrowedQueuedOrBetter"/>
    <OperandArray value="UnitQueenCocoonQueuedOrBetter"/>
</CRequirementSum>
```
