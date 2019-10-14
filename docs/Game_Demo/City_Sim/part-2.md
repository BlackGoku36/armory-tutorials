# PART-2

In `Part-2`, we will involve resources producing and collecting from buildings.

Rechecking our concept, when we place buildings, we want it to produce some resources at cost of some other resources, here the small table from the concept:

| Buildings    | Costs                      | Produce                |
| ------------ | -------------------------- | ---------------------- |
| House        | Woods, Stones, Electricity | Money, Happiness       |
| Parks        | Woods, Stones, Electricity | Money, Happiness       |
| Sawmills     | Money, Electricity         | Woods, Pollution       |
| Quarry       | Money, Electricity         | Stones, Pollution      |
| Powerplants  | Money                      | Electricity, Pollution |

---
Table of contents:
- [Producing Resources](docs/Game_Demo/City_Sim/part-2#producing-resources)

---

# PRODUCING RESOURCES

Buildings will produce given amount of resources in given interval of time. We want to make:
- House produce 5 money every 5 sec at cost of 10 woods and 10 stones.
- Sawmill produce 5 woods every 5 sec at cost of 10 money.
- Quarry produce 5 stones every 5 sec at cost of 10 money.
- Powerplant produce 10 electricity every 5 sec at cost of 20 money.
- Electricity to be used by house, sawmill, quarry to produce their resources, if they don't get enough electricity, they stop production.

!> W.I.P