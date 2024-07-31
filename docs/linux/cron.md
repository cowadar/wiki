# Cron

Een CRON-expressie is gewoon een tekenreeks die bestaat uit zes velden die elk een specifieke tijdseenheid definiëren.

Ze zijn geschreven in het volgende formaat:

```cron
{second} {minute} {hour} {day} {month} {day of the week}
```

## Waardes

De volgende waarden zijn toegestaan binnen elke tijdelijke aanduiding voor een datum/tijd-eenheid.

| Field | Allowed Values | Description |
|---|---|---|
| {second} | 0-59 | Trigger every {second} second(s) |
| {minute} | 0-59 | Trigger every {minute} minute(s) |
| {hour} | 0-23 | Trigger every {hour} hour(s) |
| {day} | 1-31 | Trigger every {day} day(s) of month |
| {month} | 1-12 | Trigger every {month} month(s) |
| {day of week} | 0-6 | MON-SUN Trigger on specific {day of week} |

## Speciale Characters

Daarnaast kunt u ook de volgende speciale tekens gebruiken om meer geavanceerde uitdrukkingen te maken:

| Special Character | Description |
|---|---|
| `*` | Trigger on tick of every time unit |
| `,` | List separator |
|`–` | Specifies a range |
| `/` | Defines an increment |

## Enkele voorbeelden

`0 * * * * *` - Executes every minute
`0 0 * * * *` - Executes every hour
`0 0 0 * * *` - Executes every day
`0 0 0 0 * *` - Executes every month
`0 0 0 1 1 *` - Executes on first day of Jan each year
`30 20 * * SAT` - Executes at 08:30pm every Saturday
`30 20 * * 6` - Executes at 08:30pm every Saturday
`0 */5 * * * *` - Executes every five minutes
`0 0 8-10/1 * * *` - Executes every hour between 8am and 10am