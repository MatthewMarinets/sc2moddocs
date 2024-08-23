# Upgrade Data
Defines upgrades, which are player-wide modifications to other sorts of data.

All entries within the Upgrade Data catalog are of tag type `CUpgrade`.

<details>
<summary>Table of Contents</summary>

- [Upgrade Data](#upgrade-data)
  - [Fields](#fields)
    - [Name](#name)
    - [Race](#race)
    - [EditorCategories](#editorcategories)
    - [EffectArray](#effectarray)
    - [EffectArrayTemplate](#effectarraytemplate)
    - [AffectedUnitArray](#affectedunitarray)
    - [Icon](#icon)
    - [Alert](#alert)
    - [InfoTooltipPriority](#infotooltippriority)
    - [TechAliasArray](#techaliasarray)
    - [DataCollection](#datacollection)
    - [MaxLevel](#maxlevel)
    - [BonusTimePerLevel](#bonustimeperlevel)
    - [BonusResourcePerLevel](#bonusresourceperlevel)
    - [LevelButton](#levelbutton)
    - [LevelRequirements](#levelrequirements)
    - [LeaderAlias](#leaderalias)
    - [LeaderLevel](#leaderlevel)
    - [LeaderPriority](#leaderpriority)
    - [UnitAllowed](#unitallowed)
    - [UnitDisallowed](#unitdisallowed)
    - [EnumExcludedUserFlags](#enumexcludeduserflags)
    - [EnumRequiredUserFlags](#enumrequireduserflags)
    - [ScoreAmount](#scoreamount)
    - [ScoreCount](#scorecount)
    - [ScoreResult](#scoreresult)
    - [ScoreValue](#scorevalue)
    - [WebPriority](#webpriority)
</details>

## Fields
### Name
* attribute `value`: string
* Overrides the gamestrings.txt key used to name this upgrade.
* Example: `<Name value="Upgrade/Name/MyUpgradeName"/>`

### Race
* attribute `value`: enum -- Neut | Prot | Terr | Zerg
* Defines what race this upgrade is associated with in the editor
* Example: `<Race value="Prot"/>`

### EditorCategories
* attribute `value`: string -- editorcategories format
  * Formatted as a series of comma-separated `key:value` pairs
* Defines the categories used to organize, search, and filter the upgrade in the editor
* Example: `<EditorCategories value="Race:Terran,UpgradeType:ArmorBonus"/>`

### EffectArray
* attribute `Reference`: upgradeable field reference
* attribute `Value`: The modification argument
* attribute `Operation`: enum -- AddBaseMultiply | Divide | Multiply | Set | Subtract | SubtractBaseMultiply
  * When unspecified, behaviour is to add
  * "Set" can be used to modify non-numeric fields
  * "Divide" and "Multiply" modify current values at the time the upgrade is applied
  * "AddBaseMultiply" and "SubtractBaseMultiply" modify a multiplier on the base value before additions and subtractions
* Array type; can appear multiple times
* Modifies the data field for the player when this upgrade is applied
  * Fields must be upgradeable
* Example 1: `<EffectArray Reference="Weapon,AP_GaussRifle,Range" Value="1"/>`
* Example 2: `<EffectArray Operation="Set" Reference="Button,AP_Thor,Tooltip" Value="Button/Tooltip/AP_ThorUpgraded"/>`

### EffectArrayTemplate
* attribute `Reference`: string expression evaluating to upgradeable field reference
* attribute `Value`: string expression evaluating to the modification argument
* attribute `Operation`: enum -- AddBaseMultiply | Divide | Multiply | Set | Subtract | SubtractBaseMultiply
* Array type; can appear multiple times
* String expressions can take extra tokens that get evaluated:
  * `^ParamId^` evaluates to the ID of any data collection that uses this upgrade
  * `^ParamLevel^` valuates to the current level of upgrade
  * `{expression}` evaluates the expression in the braces, allowing for data references or math expressions
* Extra information sourced from [sc2mapster wikia site](https://sc2mapster.fandom.com/wiki/Data/Upgrades#(None):_Effect_Array_Template)
* Example: `<EffectArrayTemplate Reference="Effect,^ParamId^@Weapon,Amount" Value="{DataCollection,^ParamId^,UpgradeInfoWeapon.DamagePerLevel+3}"/>`

### AffectedUnitArray
* attribute `value`: `CUnit` id
* Array type; can appear multiple times
* Specifies a list of units whose actors will get sent a message on upgrade completion
  * Subscribe to the message in actordata with the `Upgrade.*` signals
* Example: `<AffectedUnitArray value="Marine"/>`

### Icon
* attribute `value`: filepath
* The icon to use for the upgrade complete alert
* Example: `<Icon value="Assets\Textures\btn-tips-hacking.dds"/>`

### Alert
* attribute `value`: `CAlert` id
* Alert that plays when the upgrade is complete
* Example: `<Alert value="AICommAttackAck"/>`

### InfoTooltipPriority
* attribute `value`: integer
* Position priority in the alerts section on upgrade completion
* Example: `<InfoTooltipPriority value="5"/>`

### TechAliasArray
* attribute `value`: string
* Array type; can appear multiple times
* Alias for the upgrade that can be used to reference mutiple upgrades at once from a requirement node
* Example: `<TechAliasArray value="InfestorAbilities"/>`

### DataCollection
* attribute `value`: `CDataCollection` id
  * From the Data Collection Data catalog
* Example: `<DataCollection value="Decal_Spray_0047_01"/>`

### MaxLevel
* attribute `value`: integer
* Example: `<MaxLevel value="3"/>`

### BonusTimePerLevel
* attribute `value`: float
* Upgradeable
* For an upgrade with multiple levels, controls how much research time is added per level
* Example: `<BonusTimePerLevel value="12.5"/>`

### BonusResourcePerLevel
* attribute `value`: integer
* attribute `index`: enum -- Minerals | Vespene | Terrazine | Custom
* Upgradeable
* Can appear on the same upgrade once for each index
* For an upgrade with multiple levels, controls how much more each additional level costs
* Example: `<BonusResourcePerLevel index="Minerals" value="100"/>`

### LevelButton
* attribute `value`: `CButton` id
* Array type; can appear multiple times
* For an upgrade with multiple levels, controls the icons for each additional level
* Example: `<LevelButton value="zergflyerarmor1"/>`

### LevelRequirements
* attribute `value`: `CRequirement` id
* Upgradeable
* Array type; can appear multiple times
* For an upgrade with multiple levels, controls the requirements for each additional level
* Example: `<LevelRequirements value="AP_HaveInfestorInfestedTerran"/>`

### LeaderAlias
* attribute `value`: `CUpgrade` id
* Used for spectator views
* Example: `<LeaderAlias value="Stimpack"/>`

### LeaderLevel
* attribute `value`: integer
* Used for spectator views, number appearing on the upgrade icon in the leader panel
* Example: `<LeaderLevel value="1"/>`

### LeaderPriority
* attribute `value`: integer
* Used for spectator views, sorting priority
* Example: `<LeaderPriority value="3"/>`

### UnitAllowed
* attribute `value`: `CUnit` id
* Array type; can appear multiple times
* Enables the use of a unit when the upgrade is complete
* Example: `<UnitAllowed value="Firebat"/>`

### UnitDisallowed
* attribute `value`: `CUnit` id
* Array type; can appear multiple times
* Disables the use of a unit when the upgrade is complete
* Example: `<UnitDisallowed value="Firebat"/>`

### EnumExcludedUserFlags
* attribute `index`: enum --
  * Unit
  * ArmyUnit
  * Hero
  * Building
  * Ancient
  * Ward
  * Item
  * Powerup
  * Projectile
  * Destructible
  * Tree
  * Bridge
  * CanSleep
  * User14
  * User15
  * User16
* attribute `value`: 0 | 1
* Can appear on the same upgrade once for each index
* Example: `<EnumExcludedUserFlags index="Hero" value="1"/>`

### EnumRequiredUserFlags
* attribute `index`: enum -- (same as [EnumExcludedUserFlags](#EnumExcludedUserFlags))
* attribute `value`: 0 | 1
* Can appear on the same upgrade once for each index
* Example: `<EnumRequiredUserFlags index="Hero" value="1"/>`

### ScoreAmount
* attribute `value`: integer
* Amount of score completing this upgrade gives
* Example: `<ScoreAmount value="100"/>`

### ScoreCount
* attribute `value`: `CScoreValue` id
* Controls what score counter this upgrade's score counts towards in the replay tab
* Example: `<ScoreAmount value="ArmorTechnologyCount"/>`

### ScoreResult
* attribute `value`: `CScoreResult` id
* Controls what category of the score screen the upgrade shows up in
* Example: `<ScoreAmount value="ArmorTechnologyCount"/>`

### ScoreValue
* attribute `value`: `CScoreValue` id
* Controls what score counter this upgrade's score counts towards in the score screen
* Example: `<ScoreAmount value="ArmorTechnologyCount"/>`

### WebPriority
* attribute `value`: integer
* Example: `<WebPriority value="3"/>`
